# 缓冲区基础

从概念上，缓冲区是包含在一个对象内的基本数据元素数组，相比于一个简单的数组，`Buffer`类包含了数据关于数据的内容和行为信息的一个对象内。提供一系列的方法来方便操作数组数据。

`Java NIO`中的`Buffer`用于和通道进行交互。如你所知，数据都是从通道中读入到缓冲区，从缓冲区写入到通道中。

缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存。这块内存被包装成`NIO Buffer`对象，并提供了一组方法，用来方便的访问该块内存。

下面是它的基础概念：

### 属性

所有的缓冲区都具有四个属性来表示关于其包含的数据元素的信息。如下：

**1. 容量（Capacity）**

缓冲区能够容纳的数据元素的最大数量，在缓冲区创建时候设定，且设定后不能改变。

你只能往里写capacity个byte、long，char等类型。一旦Buffer满了，需要将其清空（通过读数据或者清除数据）才能继续写数据往里写数据。

**2. 上界（Limit）**

缓冲区的第一个不能被读或写的元素。或者说，缓冲区中现存元素的计数。

在写模式下，Buffer的limit表示你最多能往Buffer里写多少数据。 写模式下，limit等于Buffer的capacity。

当切换Buffer到读模式时， limit表示你最多能读到多少数据。因此，当切换Buffer到读模式时，limit会被设置成写模式下的position值。换句话说，你能读到之前写入的所有数据（limit被设置成已写数据的数量，这个值在写模式下就是position）

**3. 位置（Position）**

下一个要被读或写的元素的索引。位置会自动由相应的get( )和put( )函数更新。

**4. 标记（Mark）**

一个备忘位置。调用mark( )来设定mark = postion。调用reset( )设定position = mark。标记在设定前是未定义的(undefined)。
这四个属性之间总是遵循以下关系： 0 <= mark <= position <= limit <= capacity 


*让我们来看看这些属性在实际应用中的一些例子。下图展示了一个新创建的容量为10的ByteBuffer逻辑视图：*

<div style="text-align: center;">

![](../res/chapter_2/a-4.png)
</div>

位置被设为0，而且容量和上界被设为10，刚好经过缓冲区能够容纳的最后一个字节。标记最初未定义。容量是固定的，但另外的三个属性可以在使用缓冲区时改变。

### 缓冲区常用API

下面是一些Buffer类的常用的方法：

```java
package java.nio;
public abstract class Buffer {
	public final int capacity( )
	public final int position( )
	public final Buffer position (int newPositio
	public final int limit( )
	public final Buffer limit (int newLimit)
	public final Buffer mark( )
	public final Buffer reset( )
	public final Buffer clear( )
	public final Buffer flip( )
	public final Buffer rewind( )
	public final int remaining( )
	public final boolean hasRemaining( )
	public abstract boolean isReadOnly( ); 
}
```
以上方法中，特别要注意下`isReadOnly()`方法，所有的缓冲区都是可读的，但并非所有的都可写。每个具体的缓冲区都可以通过执行`isReadOnly()`方法来标识其内容是否可以被修改。一些类型的缓冲区的内容可能并非只是存储在一个数组中，例如`MappedByteBuffer`的内容可能实际上是一个只读文件。您也可以明确的创建一个只读视图的缓冲区，以便防止对其内容进行修改。对只读的缓冲区的修改，将会导致抛出`ReadOnlyBufferException`异常。

### 存取

缓冲区管理着固定数目的数据元素，


