 对pmtest5.asm的理解
-------------------------------------
------------------------------
%include	"pm.inc"	; 常量, 宏, 以及一些说明

org	0100h
	jmp	LABEL_BEGIN

**GDT描述符表**  
    ;                            段基址,           段界限     , 属性  
LABEL_GDT:             Descriptor 0,                 0, 0		   ;空描述符  
LABEL_DESC_NORMAL:     Descriptor 0,            0ffffh, DA_DRW		   ;Normal描述符    
LABEL_DESC_CODE32:     Descriptor 0,    SegCode32Len-1, DA_C+DA_32	   ;非一致,32  
LABEL_DESC_CODE16:     Descriptor 0,            0ffffh, DA_C		   ;非一致,16  
LABEL_DESC_CODE_DEST:  Descriptor 0,  SegCodeDestLen-1, DA_C+DA_32	   ;非一致,32  
**LABEL_DESC_CODE_RING3: Descriptor 0, SegCodeRing3Len-1, DA_C+DA_32+DA_DPL3**  
LABEL_DESC_DATA:       Descriptor 0,	     DataLen-1, DA_DRW             ;Data  
LABEL_DESC_STACK:      Descriptor 0,        TopOfStack, DA_DRWA+DA_32	   ;Stack,32  
**LABEL_DESC_STACK3:     Descriptor 0,       TopOfStack3, DA_DRWA+DA_32+DA_DPL3**  
LABEL_DESC_LDT:        Descriptor 0,          LDTLen-1, DA_LDT		   ;LDT  
**LABEL_DESC_TSS:        Descriptor 0,          TSSLen-1, DA_386TSS	   ;TSS**  
**LABEL_DESC_VIDEO:      Descriptor 0B8000h,      0ffffh, DA_DRW+DA_DPL3**  

**; 门                                            目标选择子,       偏移, DCount, 属性**  
**LABEL_CALL_GATE_TEST:	Gate		  SelectorCodeDest,          0,      0, DA_386CGate + DA_DPL3**
; GDT 结束  

GdtLen		equ	$ - LABEL_GDT	; GDT长度  
GdtPtr		dw	GdtLen - 1	; GDT界限  
		dd	0		; GDT基地址  

**; GDT 选择子**  

**; END of [SECTION .gdt]**

--------------------------------------------------------

**; 堆栈段ring3**  

LABEL_STACK3:  
	times 512 db 0  
TopOfStack3	equ	$ - LABEL_STACK3 - 1  

 ---------------------------------------------------------------------------------------------
 
 
[SECTION .tss]  **TSS 任务段**  

    LABEL_TSS:
		DD	0			; Back
		DD	TopOfStack		; 0 级堆栈
		DD	SelectorStack		; 
		DD	0			; 1 级堆栈
		DD	0			; 
		DD	0			; 2 级堆栈
		DD	0			; 
		DD	0			; CR3
		DD	0			; EIP
		DD	0			; EFLAGS
		DD	0			; EAX
		DD	0			; ECX
		DD	0			; EDX
		DD	0			; EBX
		DD	0			; ESP
		DD	0			; EBP
		DD	0			; ESI
		DD	0			; EDI
		DD	0			; ES
		DD	0			; CS
		DD	0			; SS
		DD	0			; DS
		DD	0			; FS
		DD	0			; GS
		DD	0			; LDT
		DW	0			; 调试陷阱标志
		DW	$ - LABEL_TSS + 2	; I/O位图基址
		DB	0ffh			; I/O位图结束标志
TSSLen		equ	$ - LABEL_TSS

-----------------------------------

[SECTION .s16]**16位代码段,进入保护模式 第 122行到259行**   

    LABEL_BEGIN:
	mov	ax, cs
	mov	ds, ax
	mov	es, ax
	mov	ss, ax
	mov	sp, 0100h

	mov	[LABEL_GO_BACK_TO_REAL+3], ax
	mov	[SPValueInRealMode], sp

**	; 初始化数据段,..各种段,堆栈段描述符,LDT,省略**  

	; 初始化 TSS 描述符
	xor	eax, eax
	mov	ax, ds
	shl	eax, 4
	add	eax, LABEL_TSS
	mov	word [LABEL_DESC_TSS + 2], ax
	shr	eax, 16
	mov	byte [LABEL_DESC_TSS + 4], al
	mov	byte [LABEL_DESC_TSS + 7], ah

	; 为加载 GDTR 作准备
	xor	eax, eax

	; 加载 GDTR
	lgdt	[GdtPtr]

	; 关中断
	cli

	; 打开地址线A20
	in	al, 92h
	or	al, 00000010b
	out	92h, al

	; 准备切换到保护模式
	mov	eax, cr0
	or	eax, 1
	mov	cr0, eax

	; 真正进入保护模式
	jmp	dword SelectorCode32:0	
	; 执行这一句会把 SelectorCode32 装入 cs, 并跳转到 Code32Selector:0  处

---------------------------------------

**[SECTION .s32]; 32 位代码段,保护模式. 由实模式跳入.**  

LABEL_SEG_CODE32:  
	mov	ax, SelectorData  
	mov	ds, ax			; 数据段选择子  
	mov	ax, SelectorVideo  
	mov	gs, ax			; 视频段选择子  
	mov	ax, SelectorStack  
	mov	ss, ax			; 堆栈段选择子  
    mov	esp, TopOfStack

**; 下面显示一个字符串**  
	mov	ah, 0Ch			; 0000: 黑底    1100: 红字  
	xor	esi, esi  
	xor	edi, edi  
	mov	esi, OffsetPMMessage	; 源数据偏移  
	mov	edi, (80 * 10 + 0) * 2	; 目的数据偏移。屏幕第  10 行, 第 0 列。  
	cld  
    .1:  
	lodsb  
	test	al, al  
	jz	.2  
	mov	[gs:edi], ax  
	add	edi, 2  
	jmp	.1  
.2:	; 显示完毕  

	call	DispReturn

**; Load  加载TSS**  

	mov	ax, SelectorTSS  
	ltr	 ax	;   在任务内发生特权级变换时要切换堆栈，
	               而内层堆栈的指针存放在当前任务的TSS中，所以要设置任务状态段寄存器 TR。

--------------------------
**指令LTR  **  
LTR（Load task register）加载一个选择子操作数到任务寄存器的可见部分，这个选择子必须指定一个在GDT中的TSS描述符。LTR也用TSS中的信息来加载任务寄存器的不可见部分。LTR是一条特权指令，只能当CPL是0时才能执行这条执令。LTR一般是当操作系统初始化过程执行的，用来初始化任务寄存器。以后，任务寄存器（TR）的内容由每次任务切换来改变。

**STR（Store task register）**存储任务寄存器的可见部分到一个通用寄存器或者到一个内存的字内。STR不是特权指令。

--------------------------
	push	SelectorStack3
	push	TopOfStack3
	push	SelectorCodeRing3 **将这三个选择子压栈,当执行retf指令时就可弹出到cs,ip中**
	push	0
	retf		; Ring0 -> Ring3，历史性转移！将打印数字 '3'。

SegCode32Len	equ	$ - LABEL_SEG_CODE32
; END of [SECTION .s32]

----------------------
**  retf指令**  
远返回指令。当它执行时，处理器先从栈中弹出一个字到IP，再弹出一个字到CS。

----------------------
**; CodeRing3   第428行 到 444行**  

**LABEL_CODE_RING3:**  
	mov	ax, SelectorVideo  
	mov	gs, ax			; 视频段选择子(目的)    
	mov	edi, (80 * 14 + 0) * 2	; 屏幕第 14 行, 第 0 列。  
	mov	ah, 0Ch			; 0000: 黑底    1100: 红字  
**	mov	al, '3'       ;                         显示3  **   
	mov	[gs:edi], ax  
  
	call	SelectorCallGateTest:0	; 测试调用门（有特权级变换），将打印字母 'C'。
	jmp	$
SegCodeRing3Len	equ	$ - LABEL_CODE_RING3
; END of [SECTION .ring3]

--------------------

**[SECTION .sdest]; 调用门目标段  第349行到第367行**  

LABEL_SEG_CODE_DEST:  
	mov	ax, SelectorVideo  
	mov	gs, ax			; 视频段选择子(目的)  
	mov	edi, (80 * 12 + 0) * 2	; 屏幕第 12 行, 第 0 列。  
	mov	ah, 0Ch			; 0000: 黑底    1100: 红字  
	mov	al, 'C'  
	mov	[gs:edi], ax   **显示字符C**

; Load LDT  
	mov	ax, SelectorLDT  
 **	lldt	ax       加载LDT**

**jmp	SelectorLDTCodeA:0	; 跳入局部任务，将打印字母 'L'。**

SegCodeDestLen	equ	$ - LABEL_SEG_CODE_DEST

----------------------------------

**; CodeA (LDT, 32 位代码段) 第409行到425行**  

LABEL_CODE_A:  
	mov	ax, SelectorVideo  
	mov	gs, ax			; 视频段选择子(目的)  
mov	edi, (80 * 13 + 0) * 2	; 屏幕第 13 行, 第 0 列。  
	mov	ah, 0Ch			; 0000: 黑底    1100: 红字  
	mov	al, 'L'  
	mov	[gs:edi], ax  

	; 准备经由16位代码段跳回实模式
	jmp	SelectorCode16:0 
CodeALen	equ	$ - LABEL_CODE_A  
; END of [SECTION .la]

-----------------------

LABEL_REAL_ENTRY:		**; 从保护模式跳回到实模式就先到了这里 第370行到第390行**  
	mov	ax, cs  
	mov	ds, ax  
	mov	es, ax  
	mov	ss, ax  
    mov	sp, [SPValueInRealMode]  
	in	al, 92h		; ┓  
	and	al, 11111101b	; ┣ 关闭 A20 地址线  
	out	92h, al		; ┛  

	sti			; 开中断  

	mov	ax, 4c00h	; ┓
	int	21h		; ┛回到 DOS
; END of [SECTION .s16]


--------------------------
**LDT**  
;                                         段基址       段界限     ,   属性  
LABEL_LDT_DESC_CODEA:	Descriptor	       0,     CodeALen - 1,   DA_C + DA_32	; Code, 32 位  

LDTLen		equ	$ - LABEL_LDT

; LDT 选择子
SelectorLDTCodeA	equ	LABEL_LDT_DESC_CODEA	- LABEL_LDT + SA_TIL
; END of [SECTION .ldt]