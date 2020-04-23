# RT-Thread学习笔记（一）

本笔记内容基于RT-Thread官方[文档中心](https://www.rt-thread.org/document/site/)

## 什么是RT-Thread
1. 概述
	一款完全由国内团队开发维护的嵌入式实时操作系统（RTOS）。正演变成一个功能强大、组件丰富的物联网操作系统
2. 架构
	自下而上为
	* 内核层： RT-Thread 的核心部分，包括了内核系统中对象的实现，例如多线程及其调度、信号量、邮箱、消息队列、内存管理、定时器等
	* 组件与服务层：组件是基于 RT-Thread 内核之上的上层软件，例如虚拟文件系统、FinSH 命令行界面、网络框架、设备框架等
	* RT-Thread软件包：运行于 RT-Thread 物联网操作系统平台上，面向不同应用领域的通用软件组件，由描述信息、源代码或库文件组成
3. 特点
	1. 采用 C 语言编写，浅显易懂，方便移植
	2. Nano版本专门针对低性能系统
	3. 对比Linux体积小，成本低，功耗低
	4. 有丰富的软件包依赖
## RT-Thread Nano
	一个极简版的硬实时内核，功能包括任务处理、软件定时器、信号量、邮箱和实时调度等相对完整的实时操作系统特性。适用于各种32位ARM入门级MCU场合。
	有下载简单，代码简单，移植简单的特点。
## 内核基础
	与硬件密切相关的，实现操作系统基础的功能的程序

1. 内核功能
	* 线程调度
	* 时钟管理
	* 线程间同步
	* 线程间通信
	* 内存管理
	* I/O设备管理
	
2. RT-Thread 启动流程
	由系统启动文件开始进入RT-Thread启动文件rtthread_startup()后进入用户的main()函数
   [官方文档](https://www.rt-thread.org/document/site/programming-manual/basic/basic/#rt-thread_1)

3. 自动化初始机制
	自动初始化机制是指初始化函数不需要被显式调用，只需要在函数定义处通过宏定义的方式进行申明，就会在系统启动过程中被执行
	个人理解就是将一系列初始化函数集成在一个大的函数表中，系统开始时自动遍历表中函数并运行
	
	[官方文档](https://www.rt-thread.org/document/site/programming-manual/basic/basic/#rt-thread_3)
	
4. 内核对象管理

   具体内核对象结构体参考等[官方文档](https://www.rt-thread.org/document/site/programming-manual/basic/basic/#rt-thread_3)

   * 所有内核对象分为静态对象与动态对象，动态对象参数多为指针，命名由_t结尾
   * 常见对象有线程，信号量，互斥量，定时器等

5. 内核配置

   通过修改工程目录下的 rtconfig.h实现

   具体配置见[官方文档](https://www.rt-thread.org/document/site/programming-manual/basic/basic/#rt-thread_5)