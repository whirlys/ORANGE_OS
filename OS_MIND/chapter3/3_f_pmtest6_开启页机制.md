
## 开启页机制

PageDirBase		equ	200000h	; 页目录开始地址: 2M
PageTblBase		equ	201000h	; 页表开始地址: 2M+4K

org	0100h
	jmp	LABEL_BEGIN

[SECTION .gdt]
; GDT
;                            段基址,       段界限, 属性
LABEL_GDT:           Descriptor 0,              0, 0     		; 空描述符
LABEL_DESC_NORMAL:   Descriptor 0,         0ffffh, DA_DRW		; Normal 描述符

**LABEL_DESC_PAGE_DIR: Descriptor PageDirBase, 4095, DA_DRW;Page Directory** ;
**LABEL_DESC_PAGE_TBL: Descriptor PageTblBase, 1023, DA_DRW|DA_LIMIT_4K;Page Tables**


GdtLen		equ	$ - LABEL_GDT	; GDT长度
GdtPtr		dw	GdtLen - 1	; GDT界限
		dd	0		; GDT基地址

; GDT 选择子
SelectorNormal		equ	LABEL_DESC_NORMAL	- LABEL_GDT

**SelectorPageDir		equ	LABEL_DESC_PAGE_DIR	- LABEL_GDT** 页目录选择子
**SelectorPageTbl		equ	LABEL_DESC_PAGE_TBL	- LABEL_GDT** 页表选择子


------------------------
-----启动分页机制 ----------------------

SetupPaging:
	; 为简化处理, 所有线性地址对应相等的物理地址.

### 首先初始化页目录

	mov	ax, SelectorPageDir	; 此段首地址为 PageDirBase
	mov	es, ax
	mov	ecx, 1024		;** 共 1K 个表项,循环1024次,将1024个页表的地址分别填进页目录中**
	xor	edi, edi
	xor	eax, eax
	mov	eax, PageTblBase | PG_P  | PG_USU | PG_RWW
.1:

	stosd	#此命令把AL或AX中的数据装入ES:DI指向的单元,然后根据DF标志将DI递增或递减
	add	eax, 4096		; 为了简化, 所有页表在内存中是连续的.
	loop	.1

### 再初始化所有页表 (1K 个, 4M 内存空间)

	mov	ax, SelectorPageTbl	; 此段首地址为 PageTblBase
	mov	es, ax
	mov	ecx, 1024 * 1024	; 共 1M 个页表项, 也即有 1M 个页,每个页是4K大小,总的包含4G
	xor	edi, edi
	xor	eax, eax
	mov	eax, PG_P  | PG_USU | PG_RWW
.2:
	stosd
	add	eax, 4096		; 每一页指向 4K 的空间
	loop	.2

	mov	eax, PageDirBase		;将页目录地址 加载进 CR3寄存器
	mov	cr3, eax
	mov	eax, cr0			;**将CR0的页机制标志设为 1 ,表明开启了页机制**
	or	eax, 80000000h
	mov	cr0, eax
	jmp	short .3
.3:
	nop

	ret
; 分页机制启动完毕 ----------------------------------------------------------

