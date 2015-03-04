# lab0 SPOC思考题

## 个人思考题

---

能否读懂ucore中的AT&T格式的X86-32汇编语言？请列出你不理解的汇编语言。
- 能。汇编语言课程中学习过。

>  http://www.imada.sdu.dk/Courses/DM18/Litteratur/IntelnATT.htm

虽然学过计算机原理和x86汇编（根据THU-CS的课程设置），但对ucore中涉及的哪些硬件设计或功能细节不够了解？
- 无

>   


哪些困难（请分优先级）会阻碍你自主完成lab实验？
- 代码的风格和注释
- 上课的方式
- 参考资料的组织和提供

>   

如何把一个在gdb中或执行过程中出现的物理/线性地址与你写的代码源码位置对应起来？
- 通过gcc的-g编译参数可以加入调试信息，从而知道

>   

了解函数调用栈对lab实验有何帮助？
- 可以知道数据在内存中的排列方式，方便阅读程序和调试

>   

你希望从lab中学到什么知识？
- 对操作系统原理有细致的了解，达到独立从零实现操作系统的水平

>   

---

## 小组讨论题

---

搭建好实验环境，请描述碰到的困难和解决的过程。
- 下载慢：找同学拷

> 

熟悉基本的git命令行操作命令，从github上
的 http://www.github.com/chyyuu/ucore_lab 下载
ucore lab实验
- 完成

> 

尝试用qemu+gdb（or ECLIPSE-CDT）调试lab1
- 完成

> 

对于如下的代码段，请说明”：“后面的数字是什么含义
```
/* Gate descriptors for interrupts and traps */
struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
};
```

- 说明占用的位数（bit）

> 

对于如下的代码段，
```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```

如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 0,1,2,3);
```
请问执行上述指令后， intr的值是多少？

- 首先gatedesc长度为64位，假设代码中intr实际是64位整型，setgate中进行了强制类型转换。
- 最后结果为 0xE0E0000010002，如果intr确实为32位，则应为低32位0x10002=65538

> 

请分析 [list.h](https://github.com/chyyuu/ucore_lab/blob/master/labcodes/lab2/libs/list.h)内容中大致的含义，并能include这个文件，利用其结构和功能编写一个数据结构链表操作的小C程序
```C
#include <stdio.h>
#include <list.h>

struct Ints {
  int data;
  list_entry_t le;
};

#define le2struct(ptr) to_struct((ptr), struct Ints, le)
#define to_struct(ptr, type, member) \
  ((type *)((char *)(ptr) - offsetof(type, member)))
#define offsetof(type, member) \
  ((size_t)(&((type *)0)->member))

int main() {
  struct Ints one, two, three, *now_int;
  list_entry_t *now;
  one.data = 1;
  two.data = 2;
  three.data = 3;
  list_init(&one.le);
  list_add_before(&one.le, &two.le);
  list_add_after(&one.le, &three.le);

  now = &two.le;
  while (1) {
    now_int = le2struct(now);
    printf("Current: %d\n", now_int->data);
    now = now->next;
    if (now == &two.le)
      break;
  }

  return 0;
}
```
- 输出

```
Current: 2
Current: 1
Current: 3
```

> 

---

## 开放思考题

---

是否愿意挑战大实验（大实验内容来源于你的想法或老师列好的题目，需要与老师协商确定，需完成基本lab，但可不参加闭卷考试），如果有，可直接给老师email或课后面谈。
- 无

>  

---
