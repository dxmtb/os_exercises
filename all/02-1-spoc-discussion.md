#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 1. 描述PXE的大致启动流程。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 1. 了解u-boot的功能。

## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常？
 1. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)

```
  + 采分点：说明了Linux的大致数量（上百个），说明了Linux系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
 1. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)

通过`grep -R syscall *`得到以下输出，共计22个。
```
user/libs/syscall.c:    return syscall(SYS_exit, error_code);
user/libs/syscall.c:    return syscall(SYS_fork);
user/libs/syscall.c:    return syscall(SYS_wait, pid, store);
user/libs/syscall.c:    return syscall(SYS_yield);
user/libs/syscall.c:    return syscall(SYS_kill, pid);
user/libs/syscall.c:    return syscall(SYS_getpid);
user/libs/syscall.c:    return syscall(SYS_putc, c);
user/libs/syscall.c:    return syscall(SYS_pgdir);
user/libs/syscall.c:    syscall(SYS_lab6_set_priority, priority);
user/libs/syscall.c:    return syscall(SYS_sleep, time);
user/libs/syscall.c:    return syscall(SYS_gettime);
user/libs/syscall.c:    return syscall(SYS_exec, name, argc, argv);
user/libs/syscall.c:    return syscall(SYS_open, path, open_flags);
user/libs/syscall.c:    return syscall(SYS_close, fd);
user/libs/syscall.c:    return syscall(SYS_read, fd, base, len);
user/libs/syscall.c:    return syscall(SYS_write, fd, base, len);
user/libs/syscall.c:    return syscall(SYS_seek, fd, pos, whence);
user/libs/syscall.c:    return syscall(SYS_fstat, fd, stat);
user/libs/syscall.c:    return syscall(SYS_fsync, fd);
user/libs/syscall.c:    return syscall(SYS_getcwd, buffer, len);
user/libs/syscall.c:    return syscall(SYS_getdirentry, fd, dirent);
user/libs/syscall.c:    return syscall(SYS_dup, fd1, fd2);
```

文件操作：SYS_open SYS_close SYS_read SYS_write SYS_seek SYS_fstat SYS_fsync SYS_getcwd SYS_getdirentry
进程管理：SYS_exit SYS_fork SYS_wait SYS_yield SYS_kill SYS_getpid SYS_lab6_set_priority SYS_sleep SYS_exec
内存管理：SYS_pgdir 
其它：SYS_putc（输出） SYS_gettime（获取时间）SYS_dup（输出输入重定向）

 ```
  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)

objdump可以显示object文件各个section的数据、汇编代码、反汇编结果、对应的高级语言的代码（需要加入调试信息）等等。
nm可以打印object文件中的符号表。
file可以输出有关文件类型的信息。

使用`objdump -D a.out`可以反汇编目标文件，在名为main的section下找到了程序中的汇编代码。程序中进行了0x80号中断，这是一个软中断，目的是进行系统调用。

eax：系统调用号，0x4为SYS_write
ebx：file descriptor号，0x1为stdout（抽象为一个文件）
ecx：write的第一个参数，写入部分的头指针
edx：write的第二个参数，写入部分的长度

使用`nm a.out`可以打印符号表，摘录如下

```
0000000000601038 d hello
0000000000601045 D main
0000000000000006 a SYS_close
000000000000003f a SYS_dup2
000000000000000b a SYS_execve
```

可以看到除了程序中定义的符号，还有很多其它的符号，是程序的其它部分使用的。

使用`file a.out`，输出如下

```
a.out: ELF 64-bit LSB  executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=1d6484d65fc385b545ae121918d39cb8dec0ff52, not stripped
```

 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)
 
strace的作用是运行程序并且输出程序运行过程中调用的系统调用。

输出如下
```
hello world
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 26.00    0.000176          22         8           mmap
 15.07    0.000102          26         4           mprotect
 11.82    0.000080          27         3           fstat
  9.01    0.000061          20         3         3 access
  7.98    0.000054          27         2           open
  6.20    0.000042          21         2           close
  5.61    0.000038          38         1           read
  4.87    0.000033          33         1           munmap
  4.43    0.000030          30         1           execve
  3.40    0.000023          23         1           brk
  3.10    0.000021          21         1           arch_prctl
  2.51    0.000017          17         1           write
------ ----------- ----------- --------- --------- ----------------
100.00    0.000677                    28         3 total
```
mmap负责分配内存，mprotect设置内存保护，fstat返回文件的信息，access检查文件权限，open、close、read、write负责进行对文件相应的操作，munmap映射文件到内存。注意到hello world程序中并没有操作磁盘上的文件，有文件操作的原因是Linux中各种设备都被抽象为文件，包括标准输入输出。还有一些文件访问是访问库文件。execve执行的就是本文件，可执行文件是在strace执行的。brk(0)返回data段的地址，arch_prctl设置进程状态。

 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
 1. 以ucore lab8的answer为例，尝试修改并运行代码，分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 
