# lec6 SPOC思考题


NOTICE
- 有"w3l2"标记的题是助教要提交到学堂在线上的。
- 有"w3l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题
---

（1） (w3l2) 请简要分析64bit CPU体系结构下的分页机制是如何实现的
```
  + 采分点：说明64bit CPU架构的分页机制的大致特点和页表执行过程
  - 答案没有涉及如下3点；（0分）
  - 正确描述了64bit CPU支持的物理内存大小限制（1分）
  - 正确描述了64bit CPU下的多级页表的级数和多级页表的结构或反置页表的结构（2分）
  - 除上述两点外，进一步描述了在多级页表或反置页表下的虚拟地址-->物理地址的映射过程（3分）
 ```
- 64bit cpu可以寻址的空间为2^64字节，即支持的物理内存大小为2^36G.为了减少页表占用的空间大小，64bit cpu体系结构会采用多级页表和反置页表的结构。
多级页表的级数不一定，但对于64bit cpu来说，通常需要多于三级。
多级页表的结构为，由若干个层级的表项组成，每层的表项中通过原始虚拟地址中的某一段作为索引找到对应的表项内容，然后通过这些内容指向下一表项。层层指向，直到最后一层中保存着该虚拟内存对应的物理内错。因为在64bit cpu中使用到的虚拟内存是远少于总虚拟内存数量的，所以采用多级页表可以节约页表存储的空间。
反向页表是每个物理内存对应一行页表，页表数量不会因为进程调用而改变。映射方式是使用哈希表将虚拟内存哈希得到对应反向页表的索引从而得到对应的物理地址。 

>  

## 小组思考题
---

（1）(spoc) 某系统使用请求分页存储管理，若页在内存中，满足一个内存请求需要150ns (10^-9s)。若缺页率是10%，为使有效访问时间达到0.5us(10^-6s),求不在内存的页面的平均访问时间。请给出计算步骤。 

- 设不在内存的页面的平均访问时间为x us
则由题有 150*(1-10%)+x*10%=500
解得 x=3650
故不在内存的页面的平均访问时间为3650us, 即3.65ms

> 500=0.9\*150+0.1\*x

（2）(spoc) 有一台假想的计算机，页大小（page size）为32 Bytes，支持32KB的虚拟地址空间（virtual address space）,有4KB的物理内存空间（physical memory），采用二级页表，一个页目录项（page directory entry ，PDE）大小为1 Byte,一个页表项（page-table entries
PTEs）大小为1 Byte，1个页目录表大小为32 Bytes，1个页表大小为32 Bytes。页目录基址寄存器（page directory base register，PDBR）保存了页目录表的物理地址（按页对齐）。

PTE格式（8 bit） :
```
  VALID | PFN6 ... PFN0
```
PDE格式（8 bit） :
```
  VALID | PT6 ... PT0
```
其
```
VALID==1表示，表示映射存在；VALID==0表示，表示映射不存在。
PFN6..0:页帧号
PT6..0:页表的物理基址>>5
```
在[物理内存模拟数据文件](./03-2-spoc-testdata.md)中，给出了4KB物理内存空间的值，请回答下列虚地址是否有合法对应的物理内存，请给出对应的pde index, pde contents, pte index, pte contents。
```
Virtual Address 6c74
Virtual Address 6b22
Virtual Address 03df
Virtual Address 69dc
Virtual Address 317a
Virtual Address 4546
Virtual Address 2c03
Virtual Address 7fd7
Virtual Address 390e
Virtual Address 748b
```

比如答案可以如下表示：
```
Virtual Address 7570:
  --> pde index:0x1d  pde contents:(valid 1, pfn 0x33)
    --> pte index:0xb  pte contents:(valid 0, pfn 0x7f)
      --> Fault (page table entry not valid)
      
Virtual Address 21e1:
  --> pde index:0x8  pde contents:(valid 0, pfn 0x7f)
      --> Fault (page directory entry not valid)

Virtual Address 7268:
  --> pde index:0x1c  pde contents:(valid 1, pfn 0x5e)
    --> pte index:0x13  pte contents:(valid 1, pfn 0x65)
      --> Translates to Physical Address 0xca8 --> Value: 16
```


- Virtual Address 6c74:
--> pde index: 0x1B   pde contents: (valid 1, pfn 0x20)
    --> pte index: 0xe1 pte contents:(valid 1, pfn 0x61)
      --> Translates to Physical Address 0xC34 --> Value: 6   

Virtual Address 6b22:
--> pde index: 0x1A   pde contents: (valid 1, pfn 0x52)
    --> pte index: 0xc7 pte contents:(valid 1, pfn 0x47)
      --> Translates to Physical Address 0x8E2 --> Value: 1A

Virtual Address 03df:
--> pde index: 0x   pde contents: (valid 1, pfn 0x5A)
    --> pte index: 0x85 pte contents:(valid 1, pfn 0x5)
      --> Translates to Physical Address 0xBF --> Value: F

Virtual Address 69dc:
--> pde index: 0x1A   pde contents: (valid 1, pfn 0x52)
    --> pte index: 0x7f pte contents:(valid 0, pfn 0x7F)
      --> Fault (page table entry not valid)

Virtual Address 317a:
--> pde index: 0xC   pde contents: (valid 1, pfn 0x18)
    --> pte index: 0xb5 pte contents:(valid 1, pfn 0x35)
      --> Translates to Physical Address 0x6BA --> Value: 1E

Virtual Address 4546:
--> pde index: 0x11   pde contents: (valid 1, pfn 0x21)
    --> pte index: 0x7f pte contents:(valid 0, pfn 0x7F)
      --> Fault (page table entry not valid)

Virtual Address 2c03:
--> pde index: 0xB   pde contents: (valid 1, pfn 0x44)
    --> pte index: 0xd7 pte contents:(valid 1, pfn 0x57)
      --> Translates to Physical Address 0xAE3 --> Value: 16

Virtual Address 7fd7:
--> pde index: 0x1F   pde contents: (valid 1, pfn 0x12)
    --> pte index: 0x7f pte contents:(valid 0, pfn 0x7F)
      --> Fault (page table entry not valid)

Virtual Address 390e:
--> pde index: 0xE   pde contents: (valid 0, pfn 0x 7F )
      --> Fault (page directory entry not valid)

Virtual Address 748b:
--> pde index: 0x1D   pde contents: (valid 1, pfn 0x)
    --> pte index: 0x7f pte contents:(valid 0, pfn 0x7F)
      --> Fault (page table entry not valid)

      

（3）请基于你对原理课二级页表的理解，并参考Lab2建页表的过程，设计一个应用程序（可基于python, ruby, C, C++，LISP等）可模拟实现(2)题中描述的抽象OS，可正确完成二级页表转换。


（4）假设你有一台支持[反置页表](http://en.wikipedia.org/wiki/Page_table#Inverted_page_table)的机器，请问你如何设计操作系统支持这种类型计算机？请给出设计方案。

 (5)[X86的页面结构](http://os.cs.tsinghua.edu.cn/oscourse/OS2015/lecture06#head-1f58ea81c046bd27b196ea2c366d0a2063b304ab)
--- 

## 扩展思考题

阅读64bit IBM Powerpc CPU架构是如何实现[反置页表](http://en.wikipedia.org/wiki/Page_table#Inverted_page_table)，给出分析报告。

--- 
