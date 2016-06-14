## 字符串处理指令

(1) lodsb、lodsw ; 把DS:SI指向的存储单元中的数据装入AL或AX，然后根据DF标志增减SI

(2) stosb、stosw ; 把AL或AX中的数据装入ES:DI指向的存储单元，然后根据DF标志增减DI

(3) movsb、movsw ; 把DS:SI指向的存储单元中的数据装入ES:DI指向的存储单元中，然后根据DF标志分别增减SI和DI

(4) scasb、scasw ; 把AL或AX中的数据与ES:DI指向的存储单元中的数据相减，影响标志位，然后根据DF标志分别增减SI和DI

(5) cmpsb、cmpsw ; 把DS:SI指向的存储单元中的数据与ES:DI指向的存储单元中的数据相减，影响标志位，然后根据DF标志分别增减SI和DI

(6) rep ; 重复其后的串操作指令。重复前先判断CX是否为0，为0就结束重复，否则CX减1，重复其后的串操作指令。   
	主要用在MOVS和STOS前。一般不用在LODS前。

上述指令涉及的寄存器：段寄存器DS和ES、变址寄存器SI和DI、累加器AX、计数器CX
           涉及的标志位：DF、AF、CF、OF、PF、SF、ZF


