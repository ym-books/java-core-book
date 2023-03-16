# 对象和变量的并发访问

本节是重点，介绍如何保证线程安全。多线程是为了高效利用cpu高效，并行处理提高程序运行效率。但是，如果不能保证得到正确的运行结果，那么这种高效也就变得毫无意义。    
因此，如何来设计线程安全的程序，则是并发编程的核心内容。

保证线程的安全，主要通过下面方式：
> - [synchronized同步方法](chapter_2_1.md)
> - [volatile关键字](chapter_2_2.md)

本章节需要掌握下面技术点：

- synchronized对象监视器为Object时的使用。
- synchronized对象监视器为Class时的使用。
- 非线程安全是怎么出现的。
- 关键字volatile的主要作用。
- 关键字synchronized和volatile的区别以及使用情况。
