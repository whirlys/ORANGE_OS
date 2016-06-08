### 借助freedos运行pmtest3.com

+ 通上两个程序步骤一样: 编译,挂载,拷贝,卸载,启动虚拟机,切换到B盘,运行程序
+ 第50页的两个小测试程序分别编译成pmtest3_1.com和pmtest3_2.com

--------------------------------------------
## 对pmtest3.asm的理解

    ;文件第20行增加了一个描述符用于描述符一个LDT

**LABEL_DESC_LDT:    Descriptor       0,        LDTLen - 1, DA_LDT	; LDT**  

-------------------------------

**文件的第116行到135行**

   ** ; 初始化 LDT 在 GDT 中的描述符,把 LDT 当做是 一个段 来看待**
   
xor	eax, eax  
	mov	ax, ds  
	shl	eax, 4  
	add	eax, LABEL_LDT  
	mov	word [LABEL_DESC_LDT + 2], ax  
	shr	eax, 16  
	mov	byte [LABEL_DESC_LDT + 4], al  
	mov	byte [LABEL_DESC_LDT + 7], ah  

**; 初始化 LDT 中的描述符**  
	xor	eax, eax  
	mov	ax, ds  
	shl	eax, 4  
	add	eax, LABEL_CODE_A  
	mov	word [LABEL_LDT_DESC_CODEA + 2], ax  
	shr	eax, 16  
	mov	byte [LABEL_LDT_DESC_CODEA + 4], al  
	mov	byte [LABEL_LDT_DESC_CODEA + 7], ah  

---------------------------------------------------

**第269行到280行,LDT表和选择子的定义**  
LABEL_LDT:  
;                            段基址       段界限      属性  
LABEL_LDT_DESC_CODEA: Descriptor 0, CodeALen - 1, DA_C + DA_32 

LDTLen		equ	$ - LABEL_LDT

; LDT 选择子
SelectorLDTCodeA	equ	LABEL_LDT_DESC_CODEA	- LABEL_LDT + **SA_TIL**
**注意,这里选择子的属性中ＴＩ位置为1,表示这是一个LDT选择子**

---------------------------------------------------


**[SECTION .s32]; 32 位代码段. 由实模式跳入   第183行到221行**.

LABEL_SEG_CODE32:  
	mov	ax, SelectorData  
	mov	ds, ax			; 数据段选择子  
	mov	ax, SelectorVideo  
	mov	gs, ax			; 视频段选择子  
	mov	ax, SelectorStack  
	mov	ss, ax			; 堆栈段选择子  
	mov	esp, TopOfStack  

**	; 下面显示一个字符串**  
mov	ah, 0Ch			; 0000: 黑底    1100: 红字  
	xor	esi, esi  
	xor	edi, edi  
	mov	esi, OffsetPMMessage	; 源数据偏移    
	mov	edi, (80 * 10 + 0) * 2	; 目的数据偏移。屏幕第 10 行, 第 0 列。  
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
    
; Load LDT   
	mov	ax, SelectorLDT    **选择子**  
	
	lldt	ax        **LLDT ax,加载LDT**

	jmp	SelectorLDTCodeA:0	
+ 因为选择子SelectorLDTCodeA的TI位是1,表示这是一个LDT的选择子所以跳到LDT中
+ 接下来运行CODEA程序

SegCode32Len	equ	$ - LABEL_SEG_CODE32
; END of [SECTION .s32]

----------------------------------------------------------------------

**CodeA (LDT, 32 位代码段),这是我们要运行的程序,显示字符串,第283行到第299行**  
[SECTION .la]  
ALIGN	32  
[BITS	32]  
LABEL_CODE_A:  
	mov	ax, SelectorVideo  
	mov	gs, ax			; 视频段选择子(目的)    
	mov	edi, (80 * 12 + 0) * 2	; 屏幕第 10 行, 第 0 列。  
	mov	ah, 0Ch			; 0000: 黑底    1100: 红字  
	mov	al, 'L'  
	mov	[gs:edi], ax  

	; 准备经由16位代码段跳回实模式
	jmp	SelectorCode16:0
CodeALen	equ	$ - LABEL_CODE_A
; END of [SECTION .la]
