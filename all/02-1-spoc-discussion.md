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
 
Linux系统调用有206个

```
fork 创建一个新进程
clone 按指定条件创建子进程
execve 运行可执行文件
exit 中止进程
_exit 立即中止当前进程
getdtablesize 进程所能打开的最大文件数
getpgid 获取指定进程组标识号
setpgid 设置指定进程组标志号
getpgrp 获取当前进程组标识号
setpgrp 设置当前进程组标志号
getpid 获取进程标识号
getppid 获取父进程标识号
getpriority 获取调度优先级
setpriority 设置调度优先级
modify_ldt 读写进程的本地描述表
nanosleep 使进程睡眠指定的时间
nice 改变分时进程的优先级
pause 挂起进程，等待信号
personality 设置进程运行域
prctl 对进程进行特定操作
ptrace 进程跟踪
sched_get_priority_max 取得静态优先级的上限
sched_get_priority_min 取得静态优先级的下限
sched_getparam 取得进程的调度参数
sched_getscheduler 取得指定进程的调度策略
sched_rr_get_interval 取得按RR算法调度的实时进程的时间片长度
sched_setparam 设置进程的调度参数
sched_setscheduler 设置指定进程的调度策略和参数
sched_yield 进程主动让出处理器,并将自己等候调度队列队尾
vfork 创建一个子进程，以供执行新程序，常与execve等同时使用
wait 等待子进程终止
wait3 参见wait
waitpid 等待指定子进程终止
wait4 参见waitpid
capget 获取进程权限
capset 设置进程权限
getsid 获取会晤标识号
setsid 设置会晤标识号
fcntl 文件控制
open 打开文件
creat 创建新文件
close 关闭文件描述字
read 读文件
write 写文件
readv 从文件读入数据到缓冲数组中
writev 将缓冲数组里的数据写入文件
pread 对文件随机读
pwrite 对文件随机写
lseek 移动文件指针
_llseek 在64位地址空间里移动文件指针
dup 复制已打开的文件描述字
dup2 按指定条件复制文件描述字
flock 文件加/解锁
poll I/O多路转换
truncate 截断文件
ftruncate 参见truncate
umask 设置文件权限掩码
fsync 把文件在内存中的部分写回磁盘
access 确定文件的可存取性
chdir 改变当前工作目录
fchdir 参见chdir
chmod 改变文件方式
fchmod 参见chmod
chown 改变文件的属主或用户组
fchown 参见chown
lchown 参见chown
chroot 改变根目录
stat 取文件状态信息
lstat 参见stat
fstat 参见stat
statfs 取文件系统信息
fstatfs 参见statfs
readdir 读取目录项
getdents 读取目录项
mkdir 创建目录
mknod 创建索引节点
rmdir 删除目录
rename 文件改名
link 创建链接
symlink 创建符号链接
unlink 删除链接
readlink 读符号链接的值
mount 安装文件系统
umount 卸下文件系统
ustat 取文件系统信息
utime 改变文件的访问修改时间
utimes 参见utime
quotactl 控制磁盘配额
ioctl I/O总控制函数
_sysctl 读/写系统参数
acct 启用或禁止进程记账
getrlimit 获取系统资源上限
setrlimit 设置系统资源上限
getrusage 获取系统资源使用情况
uselib 选择要使用的二进制函数库
ioperm 设置端口I/O权限
iopl 改变进程I/O权限级别
outb 低级端口操作
reboot 重新启动
swapon 打开交换文件和设备
swapoff 关闭交换文件和设备
bdflush 控制bdflush守护进程
sysfs 取核心支持的文件系统类型
sysinfo 取得系统信息
adjtimex 调整系统时钟
alarm 设置进程的闹钟
getitimer 获取计时器值
setitimer 设置计时器值
gettimeofday 取时间和时区
settimeofday 设置时间和时区
stime 设置系统日期和时间
time 取得系统时间
times 取进程运行时间
uname 获取当前UNIX系统的名称、版本和主机等信息
vhangup 挂起当前终端
nfsservctl 对NFS守护进程进行控制
vm86 进入模拟8086模式
create_module 创建可装载的模块项
delete_module 删除可装载的模块项
init_module 初始化模块
query_module 查询模块信息
*get_kernel_syms 取得核心符号,已被query_module代替
brk 改变数据段空间的分配
sbrk 参见brk
mlock 内存页面加锁
munlock 内存页面解锁
mlockall 调用进程所有内存页面加锁
munlockall 调用进程所有内存页面解锁
mmap 映射虚拟内存页
munmap 去除内存页映射
mremap 重新映射虚拟内存地址
msync 将映射内存中的数据写回磁盘
mprotect 设置内存映像保护
getpagesize 获取页面大小
sync 将内存缓冲区数据写回硬盘
cacheflush 将指定缓冲区中的内容写回磁盘
getdomainname 取域名
setdomainname 设置域名
gethostid 获取主机标识号
sethostid 设置主机标识号
gethostname 获取本主机名称
sethostname 设置主机名称
socketcall socket系统调用
socket 建立socket
bind 绑定socket到端口
connect 连接远程主机
accept 响应socket连接请求
send 通过socket发送信息
sendto 发送UDP信息
sendmsg 参见send
recv 通过socket接收信息
recvfrom 接收UDP信息
recvmsg 参见recv
listen 监听socket端口
select 对多路同步I/O进行轮询
shutdown 关闭socket上的连接
getsockname 取得本地socket名字
getpeername 获取通信对方的socket名字
getsockopt 取端口设置
setsockopt 设置端口参数
sendfile 在文件或端口间传输数据
socketpair 创建一对已联接的无名socket
getuid 获取用户标识号
setuid 设置用户标志号
getgid 获取组标识号
setgid 设置组标志号
getegid 获取有效组标识号
setegid 设置有效组标识号
geteuid 获取有效用户标识号
seteuid 设置有效用户标识号
setregid 分别设置真实和有效的的组标识号
setreuid 分别设置真实和有效的用户标识号
getresgid 分别获取真实的,有效的和保存过的组标识号
setresgid 分别设置真实的,有效的和保存过的组标识号
getresuid 分别获取真实的,有效的和保存过的用户标识号
setresuid 分别设置真实的,有效的和保存过的用户标识号
setfsgid 设置文件系统检查时使用的组标识号
setfsuid 设置文件系统检查时使用的用户标识号
getgroups 获取后补组标志清单
setgroups 设置后补组标志清单
ipc 进程间通信总控制调用
sigaction 设置对指定信号的处理方法
sigprocmask 根据参数对信号集中的信号执行阻塞/解除阻塞等操作
sigpending 为指定的被阻塞信号设置队列
sigsuspend 挂起进程等待特定信号
signal 参见signal
kill 向进程或进程组发信号
*sigblock 向被阻塞信号掩码中添加信号,已被sigprocmask代替
*siggetmask 取得现有阻塞信号掩码,已被sigprocmask代替
*sigsetmask 用给定信号掩码替换现有阻塞信号掩码,已被sigprocmask代替
*sigmask 将给定的信号转化为掩码,已被sigprocmask代替
*sigpause 作用同sigsuspend,已被sigsuspend代替
sigvec 为兼容BSD而设的信号处理函数,作用类似sigaction
ssetmask ANSI C的信号处理函数,作用类似sigaction
msgctl 消息控制操作
msgget 获取消息队列
msgsnd 发消息
msgrcv 取消息
pipe 创建管道
semctl 信号量控制
semget 获取一组信号量
semop 信号量操作
shmctl 控制共享内存
shmget 获取共享内存
shmat 连接共享内存
shmdt 拆卸共享内存
功能分类
进程控制
文件系统控制
系统控制
内存管理
网络管理
socket控制
用户管理
进程间通信
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

系统调用是用户程序与操作系统交互的接口，本例中使用的是write系统调用，作用是向指定文件描述符写入数据。

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

程序调用printf以后，printf最终会调用系统调用sys_write
此时CPU收到软中断0x80，转操作系统中断处理程序，0x80为系统调用
系统调用处理完成后，就会返回原程序

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
 1. 以ucore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 
