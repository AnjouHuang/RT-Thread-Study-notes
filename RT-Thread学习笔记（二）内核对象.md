#  RT-Thread学习笔记（二）内核对象
本笔记基于[官方文档](https://www.rt-thread.org/document/site/programming-manual/basic/basic/#rt-thread_1)书写

* 内容涉及线程(Thread),时钟(Timer),进程间通信（IPC,包括信号量、互斥量、邮件，队列），内存，与中断
* 官方文档中对每个部分的实例代码都可在[此处](https://www.rt-thread.org/document/site/tutorial/quick-start/stm32f103-simulator/stm32f103-simulator/)的工程中kernel-sample文件夹中找到，可快速模拟运行
* 带超链接的标题可直接点击进入官方文档，一些具体的定义将不再重复阐述

## [线程](https://www.rt-thread.org/document/site/programming-manual/thread/thread/)

	我所理解的线程就是一个个执行任务的小程序，根据优先级和抢占时间分别占用处理器
	线程执行时需要运行环境，称为上下文，具体来说就是各个变量和数据，包括所有的寄存器变量、堆栈、内存信息

### 线程的管理方式
	抢占式，优先级高的先运行，优先级低的被挂起，优先级相同则按时间片属性轮流运行
### 线程工作机制
#### 线程控制块
	见官方文档
#### 重要属性
#### 线程栈
	用于保持线程切换时的上下文信息，有入口函数（PC 寄存器）、入口参数（R0 寄存器）、返回位置（LR 寄存器）、当前机器运行状态（CPSR 寄存器）值得关注
#### 线程状态
	分初始、就绪、运行、挂起、关闭状态，便于线程管理
	实际上就绪状态和运行状态是等同的
#### 优先级
	决定线程被调度的优先级，RT-Thread最大支持256个优先级，0为最高
#### 时间片
	时间片仅对优先级相同的就绪态线程有效。单位是一个系统节拍。
	同优先级线程独占处理器的时间与时间片成正比例关系
#### 入口函数
	实现线程的具体功能，及完成你想让线程完成的事
#### 线程错误码
	类型见文档
#### 线程状态切换
	通过一系列函数实现
	1. rt_thread_create/init() 进入到初始状态
	2. rt_thread_startup() 进入到就绪状态
	3. 就绪状态的线程被调度器调度后进入运行状态
	4. 当处于运行状态的线程调用 rt_thread_delay()，rt_sem_take()，rt_mutex_take()，rt_mb_recv() 等函数或者获取不到资源时，将进入到挂起状态
	5. 处于挂起状态的线程，如果等待超时依然未能获得资源或由于其他线程释放了资源，那么它将返回到就绪状态
	6. 挂起状态的线程rt_thread_delete/detach() 函数，将更改为关闭状态
	7. 运行状态的线程，如果运行结束，就会在线程的最后部分执行 rt_thread_exit() 函数，将状态更改为关闭状态
### 线程的管理
* 分为动态线程和静态线程的管理，两种不同类型的线程对应不同的管理函数
* 具体的函数说明都可在[官方文档](https://www.rt-thread.org/document/site/programming-manual/thread/thread/#_16)中可自行查阅
* 在此仅罗列一些特点，方便理解，这些特点广泛使用于内核对象，在以后的内容中不再阐述
	1. 动态对象定义中多为指针,且函数名以’_t‘结尾
	2. 动态对象的创建删除对应后缀为“creat”,'delete',静态的为“init”,'detach'
## [时钟](https://www.rt-thread.org/document/site/programming-manual/timer/timer/)
* 该部分十分简单，官方文档十分容易理解
* 注意硬件定时器与软件定时器的区别
	硬件定时器以中断方式实现定时，软件定时器由操作系统提供，时钟节拍（OS Tick）的时间长度为单位
* 值得一提的是高精度延时的方法
## [线程间同步](https://www.rt-thread.org/document/site/programming-manual/ipc1/ipc1/)
	详细内容见文档应思考如下问题
	1.信号量，互斥量对共有资源的占用特点
	2.事件集的触发方式
## [线程间通信](https://www.rt-thread.org/document/site/programming-manual/ipc2/ipc2/)
### 邮箱
* 4K大小，数据过大用指针传
###  消息队列
* 邮箱的扩展
* FIFO原则传递信息
### 信号
* 本质是软中断
* 进程安装信号后，在接受到信号后，执行信号指定的信号中断函数
## [内存管理](https://www.rt-thread.org/document/site/programming-manual/memory/memory/)
	了解三种内存管理模式与特点
	小内存管理算法，slab管理算法，menheap管理算法
## [中断管理](https://www.rt-thread.org/document/site/programming-manual/interrupt/interrupt/)
	见文档





















