## 编译kernel错误： '__stack_chk_fail'
***from 百度空间 碧海蓝天***

Ubuntu Linux上编译kernel出错__stack_chk_fail

init/built-in.o: In function `try_name':

do_mounts.c.text+0x5e3)：对‘__stack_chk_fail’未定义的引用
init/built-in.o: In function `name_to_dev_t':

(.text+0x8cb)：对‘__stack_chk_fail’未定义的引用
init/built-in.o: In function `change_floppy':

(.init.text+0xa11)：对‘__stack_chk_fail’未定义的引用
init/built-in.o: In function `mount_block_root':

(.init.text+0xca7)：对‘__stack_chk_fail’未定义的引用
init/built-in.o: In function `do_header':

initramfs.c.init.text+0x4343)：对‘__stack_chk_fail’未定义的引用
arch/i386/kernel/built-in.o.text+0x54c6): more undefined references to `__stack_chk_fail' follow
make[1]: *** [.tmp_vmlinux1] 错误 1

make[1]: Leaving directory `/usr/src/linux-2.6.17.10'
make: *** [debian/stamp-build-kernel] 错误 2

### 在顶层的Makefile里找到CFLAGS然后添加  -fno-stack-protector 标志！

其实这是传给GCC的一个编译选项。

### -fno-stack-protector参数用来disable Stack-smashing protection 

Ubuntu 6.10中，gcc默认用-fstack-protector参数进行编译，很不友好的东东


