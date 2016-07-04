## 利用 0E820h int 15h 中断获取内存信息 

在启动分页机制之前，我们要设置页目录和页表信息，理论上，我们利用一页内存(4k)来存放页目录，用1k页(4M)来存放页表，可以表示4G的内存。但是我们的内存不一定就是固定4G的，可能是1G，512MB或者更小，而且除了要知道内存容量的大小，我们更想知道各段内存地址的type，因为编程时，属性为reserved的段不能被程序分配使用。

基于以上问题，我们在启动分页机制之前，利用0E820h int 15h 中断获取内存信息，以便进行有效合理的设置。下面是一种使用int 15h的方法。因为是利用BIOS中断，所以要在进入保护模式之前调用，因为进入保护模式后中断向量表会被重新设置。
 
 -------------------
### 程序示例
 ```
_MemChkBuf:        times    256   db     0

    ;得到内存数
    mov    ebx, 0  
    mov    di, _MemChkBuf
.loop:

    mov    eax, 0e820h  ;要获取内存需要将eax设为 0e820h
    mov    ecx, 0534D4150h
    int    15h
    jc     LABEL_MEM_CHK_FAIL
    add    di, 20
    inc    dword [_dwMCRNumber]
    cmp    ebx, 0
    jne    .loop
    jmp    LABEL_MEM_CHK_OK
LABEL_MEM_CHK_FAIL:

    mov    dword [_dwMCRNumber], 0
LABEL_MEM_CHK_OK:
 
 
 --------------------------
调用中断int 15h 之前，需要填充如下寄存器：

+ eax  int 15h 可以完成许多工作，主要有ax的值决定，我们想要获取内存信息，需要将ax赋值为0E820H。

+ ebx  放置着“后续值(continuation value)”，第一次调用时ebx必须为0.

+ es:di  指向一个地址范围描述结构 ARDS(Address Range Descriptor Structure), BIOS将会填充此结构。
+ ecx  es:di所指向的地址范围描述结构的大小，以字节为单位。无论es:di所指向的结构如何设置，BIOS最多将会填充ecx字节。不过，通常情况下无论ecx为多大，BIOS只填充20字节，有些BIOS忽略ecx的值，总是填充20字节。

+ edx  0534D4150h('SMAP')——BIOS将会使用此标志，对调用者将要请求的系统映像信息进行校验，这些信息被BIOS放置到es:di所指向的结构中。

------------------
    中断调用之后，结果存放于下列寄存器之中。
    
+ CF  CF=0表示没有错误，否则存在错误。

+ eax   0534D4150h('SMAP')

+ es:di  返回的地址范围描述符结构指针，和输入值相同。

+ ecx BIOS填充在地址范围描述符中的字节数量，被BIOS所返回的最小值是20字节。

+ ebx  这里放置着为等到下一个地址描述符所需要的后续值，这个值得实际形势依赖于具体的BIOS的实现，调用者不必关心它的具体形式，自需在下一次迭代时将其原封不动地放置到ebx中，就可以通过它获取下一个地址范围描述符。如果它的值为0，并且CF没有进位，表示它是最后一个地址范围描述符。