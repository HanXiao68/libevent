# libevent
多线程
！[libevent的核心代码部分](https://github.com/HanXiao68/libevent/blob/master/libevent%E6%A0%B8%E5%BF%83%E9%83%A8%E5%88%86.pdf)

memcache的源码，它的io就是一个非常典型的基于libevent的多线程io服务

因此，Libevent允许进行可移植的应用程序开发，并提供操作系统上可用的最可伸缩的事件通知机制。Libevent还可以用于多线程应用程序，方法是隔离每个事件库以便只有一个线程访问它，或者锁定对单个共享事件库的访问。

Libevent还为缓冲网络IO提供了一个复杂的框架，支持套接字、过滤器、速率限制、SSL、零拷贝文件传输和IOCP。Libevent包括对几个有用协议的支持，包括DNS、HTTP和一个最小的RPC框架。

memcache基于c语言，他的线程池基于管道，windows不支持管道

线程池和线程通信 通过管道，每个线程有一个eventbase。参考memcache。

c++中static 不需要对象就可以访问

用vector放线程池，可以下标操作，list不能下标

.h文件中不加命名空间，少加头文件。 不知道调用者的情况
头文件 头文件随便加


阻塞式

select：跨平台--Linux和window都支持。每次都要从用户空间拷贝到内核空间。 遍历整个fd_set.  接口简单，开发简单；监听的fd数量有限制。

poll：和select

epoll：只拷贝一次，共享内存交互。内部基于红黑树特点。
        不用全部复制，返回双向链表
        只通过epoll wait返回红黑树中 fd状态发生变化的。
    缺点：不支持windows

    LT水平触发：如果事件没有处理，系统一直通知。epoll wait始终能获取到未处理的链表。
    ET边沿触发：只通知一次，每当状态发生变化时，触发一个事件
    两者本质区别是：取出来后，ET不删红黑树中。下一次取还是这个。ET取完以后，就删除了
iocp：不支持linux

在libevent中的级别：epoll---poll---select--iocp(需要指定)
