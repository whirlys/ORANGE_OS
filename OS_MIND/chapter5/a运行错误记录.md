### 提示: ld: i386 architecture of input file `hello.o' is incompatible with i386:x86-64 output
根据书中的方法编译链接

; (ld 的‘-s’选项意为“strip all”)   

$ nasm -f elf hello.asm -o hello.o   
$ ld -s hello.o -o hello

+ 此时计算机提示:ld: i386 architecture of input file `hello.o' is incompatible with i386:x86-64 output

用ls查看也没有hello 文件

上网查 [发现答案](http://www.linuxquestions.org/questions/programming-9/assembly-error-i386-architecture-incompatible-with-i386-x86-64-output-827609/)

链接不成功的原因是: 代码是32位代码,而指令ld -s hello.o -o hello尝试把代码链接成我机器原本的64位代码,32位当然链接不成64位啦   
    链接指令可以改为: ** ld -m elf_i386 -s -o hello hello.o**,这时产生的文件就可以在支持i386的机器上运行了,因为使用-m elf_i386可以模拟32位平台上ld指令; 大部分的64位机器是支持32位程序的
    
-----------------
## 同样在b中链接出错:ld: i386 architecture of input file `foo.o' is incompatible with i386:x86-64 output

汇编语言用nasm编写并用nasm编译器编译，而C语言用的是gcc编译，这些都没有问题，但是在链接的时候出错了; nasm 编译产生的是32位的目标代码，gcc 在64位平台上默认产生的是64位的目标代码，这两者在链接的时候出错，gcc在64位平台上默认以64位的方式链接   

解决方案：

让gcc 产生32位的代码，并在链接的时候以32位的方式进行链接

在这种情况下只需要修改编译和链接指令即可，具体如下：

32位的编译链接指令

1 nasm -f elf foo.s  -o  foo.o   
2 gcc  -m32  -c  bar.c  -o bar.o   
3 ld  -m elf_i386 -s -o foobar foo.o bar.o   