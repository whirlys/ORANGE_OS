### 借助DOS运行程序步骤

1. 到[freedos官方网站](http://www.freedos.org/)下载一个FreeDos,解压后将其中的a.img复制到我们的工作目录中,改名为freedos.img
2. 用bximage生成一个软盘映像,起名为pm.img, **注意,此时要格式化才能使用,否则在第6步会出现问题,"mount 您必须指定文件系统的类型 "**,详见本文后半部分.
3. 修改bochsrc,确保有以下三行:   
    floppya:    1_44=freedos.img, status=inserted   
    floppyb:    1_44=pm.img, status=inserted   
    boot:   a   
4. 启动bochs,待freedos启动完毕后格式化B:盘,即使用命令:
    ** A:\>format  b **
5. 将pmtest1.asm中第8行07c00改为0100h,并重新编译   
**nasm  pmtest1.asm -o pmtest1.com**
6. 将pmtest1.com复制到软盘pm.img上   
sudo mount -o loop pm.img /mnt       ;书中是挂在到floppy下,而我直接挂载在mnt上   
sudo cp pmtest1.com /mnt
sudo umount /mnt   
7. 到freedos下执行命令:   >b:\pmtest1.com

--------------------------
#### 之所以程序pmtest1.com能运行,我的分析是:   
+ **第3步:**我们已经把freedos.img作为freedos的A盘,(即boot根,已经是一个dos系统了),而pm.img作为B盘   

+ **第6步:**把pm.img挂载在目录/mnt/floppy下,所以将二进制程序pmtest1.com复制到/mnt/floppy中就相当于把程序复制到pm.img中,然后umount   
+ **第7步:**在dos下执行:    >B:\pmtest1.com   
也就是执行了B盘(即软盘pm.img)中的pmtest1.com,**所以程序pmtest1.com是借助dos运行起来的**

-----------------
##关于可能会出现的error, "mount 您必须指定文件系统的类型" 解决方案

我们用bximage生成了pm.img以后,pm.img应该是没有文件系统的,只是一张纯软盘，还需要对其进行**格式化**成某一类型的文件系统,此处参考了[这篇博客](http://kevinlp.com/oranges-mount-error.html)   
**用命令: file pm.img 可看到pm.img的类型是data**
#### 解决步骤:
1. 写入空白内容： 
dd if=/dev/null of=pm.img bs=512 count=1 conv=notrunc    

2. 使用 losetup 命令，将 data.img 作为 loop device 使用：
sudo losetup /dev/loop0 pm.img

3. 然后，格式化这个 loop device：
sudo mkfs.msdos /dev/loop0

4. 检查文件系统：
sudo fsck.msdos /dev/loop0

5. 删除 loop device：
sudo losetup -d /dev/loop0

6. 这时候，pm.img 已经格式化完成，可以作为一个软盘镜像使用。   
用file查看，结果为：  pm.img: DOS floppy 1440k, x86 hard disk boot sector   

7. 再次输入:
sudo mount -o loop pm.img /mnt/floppy   
挂载成功！！！继续实验lo～～～

