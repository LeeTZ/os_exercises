# 操作系统概述思考题

## 个人思考题

---

分析你所认识的操作系统（Windows、Linux、FreeBSD、Android、iOS）所具有的独特和共性的功能？
- 共性功能：这些操作系统都具有图形界面和网络模块，同时，具有CPU管理功能、内存管理功能、外部设备管理功能、文件管理功能和进程管理功能。
- 特性功能：Linux是开源且免费的，它支持多用户，安全性高。Windows是封闭的操作系统，用户界面简单易懂，基于windows的软件应用数量是其余操作系统的成百上千倍。 BSD的许可证最为宽松。在手机平台上，Android开源，支持第三方应用，iOS则相对封闭，需要通过iClouds和Appstore来管理其应用。

>  

请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。
- 1、微处理器管理功能。要对系统中各个微处理器的状态进行登记，还要登记各个作业对微处理器的要求。用优化算法实现最佳调度规则。把所有的微处理器分配给各个用户作业使用。
- 2、内存管理功能。要为各个用户作业分配内存空间；保护已占内存空间的作业不被破坏；是结合硬件实现信息的物理地址至逻辑地址的变换。以方便用户的使用和操作。
- 3、外部设备管理功能。当用户要求某种设备时，应马上分配给用户所要求的设备，并按用户要求驱动外部设备以供用户应用。并且对外部设备的中断请求，设备管理模块要给以响应并处理。
- 4、文件管理功能。负责文件目录、文件组织、文件操作和文件保护。
- 5、进程管理功能。对作业执行的全过程进行管理和控制。

>   

请给出你觉得的更准确的操作系统的定义？
- 操作系统是一个对计算机硬件资源进行物理抽象，并对软件资源进行管理的计算机程序。 

>   

你希望从操作系统课学到什么知识？
- 希望理解系统运行的基本原理，为日后在上面开发应用打好理论基础。知其然并知其所以然。 

>   

---

## 小组讨论题

---

目前的台式PC机标准配置和价格？
- 据中关村在线数据，以销售最火的联想Erazer X310为例：
- CPU 系列：英特尔 酷睿i5 4代系列；CPU 型号Intel 酷睿i5 4460；CPU 频率：3.2GHz
- 内存：8GB DDR3；硬盘：1TB 5400转；光驱：DVD刻录机
- 显卡：NVIDIA GeForce GTX 750 2GB 独立显卡
- 有线网卡: 1000Mbps以太网卡
- 价格：人民币 3899~4999元
> 

你理解的命令行接口和GUI接口具有哪些共性和不同的特征？
- 共性：都可以对操作系统进行控制。
- 特性：命令行接口做批处理和自由组合的复杂操作较为高效，GUI接口直观，简单，但效率略差。  

> 

为什么现在的操作系统基本上用C语言来实现？
- 我认为第一点是效率原因，C代码的效率相较于其他面向对象的语言要高，也没有掩盖任何的内存分配机制。第二点原因应该是历史因素。C语言本身就是为了写Unix设计的语言。后来的操作系统基本都是基于Unix的。真正工业环境下，敢放弃成熟模型、另起炉灶的勇者太少了，这样做也不划算，所以越来越多的底层库和接口函数都是C形式的，因此形成了这种现象。

>  

为什么没有人用python，java来实现操作系统？
- 在上一个问题中回答过了。用什么语言来开发系统，还是得权衡运行效率、可读性、可维护性、可重用性和可扩展性等方面。在很多操作系统相关的方面，比如驱动程序的开发，完全没有必要用到面向对象的方法，更多的是考虑运行效率，因此C相对于其他高级语言是个好的选择。同样的，在真正工业环境下，敢放弃成熟模型、另起炉灶的勇者太少了，这样做也不划算，所以越来越多的底层库和接口函数都没有python，java形式的，因此形成了这种现象。

>  

请评价用C++来实现操作系统的利弊？
- 利：更加接近于人类思维，编写可能更加方便。
- 弊：工作量大。大量的底层代码是用C写的，内核高手也大都是C高手，让他们改变习惯不容易。效率不如C高。

>  

---

## 开放思考题

---

请评价微内核、单体内核、外核（exo-kernel）架构的操作系统的利弊？
- 单体内核比较简单，不需要消息传递进程切换的时间，效率高超。微内核设计方式带来的优势有：模块化的方式设计操作系统，模块的设计者只需要关注自己的功能模块；操作系统的更新时，除了微内核本身，可以动态的更新其他的功能模块；在系统运行的时候，可以根据需要动态的使能禁止对应的模块，以释放计算机的资源。外核将资源保护及其管理分割开来。由于外核只提供有限的原语，所以外核操作系统效率很高。由于进程间通信、虚拟内存管理等传统概念都是在应用层实现的，所以可以很容易对他们进行扩展、专业化和替换。

>  

请评价用LISP,OCcaml, GO, D，RUST等实现操作系统的利弊？
- 坦率的讲，我不懂题面中除了LISP之外的语言，不敢妄加评判。这个问题的答案与上面回答C，python，java的问题类似。  

>  

进程切换的可能实现思路？
- 我认为进程切换是一种切换堆栈的高级汇编程序技巧。当进程A要切换出去时，先把ESI,EDI,EBP,EIP保存到自己的堆栈中，而这个被保存的EIP实际指向了用来恢复其它三个寄存器的指令的起始地址。接着呢就进行实质的ESP的切换。其它需要切换回来的状态分量全都在新的ESP中了，用怎样的顺序来实现切换都可以了，但有一点是必须按照保存的顺序进行切换。  

>  

计算机与终端间通过串口通信的可能实现思路？
- 设置端口号，传输速度，校验位，数据位，自动发送的间隔时间以及发送的串的协议。然后选择发送的数据类型，开始发送，根据设置进行接受处理。  

>  

为什么微软的Windows没有在手机终端领域取得领先地位？
- 我认为这个更多的是时机选择和商业战略制定的问题。比如鲍尔默当时认为iPhone将是一个边缘产品；Win8开发拖慢了Winphone的推出时机；Winphone没有一个明确的战略目标，比较好用？比较便宜？比较快？比较简单操作？比较安全？还是连接性更强？不能够完全责怪操作系统本身。微软操作系统的很多概念是领先的。如扁平化设计风格，如滑动出现的Global与local Setting界面等。  

>  

你认为未来（10年内）的操作系统应该具有什么样的特征和功能？
- 大力推广云存储和分布式的概念，设备完全就是终端，做到跨平台跨设备的内容共享和一致性体验。同时处理能力更加强大。加入虚拟现实，体感交互等多种新的交互方式。 

>  

---
