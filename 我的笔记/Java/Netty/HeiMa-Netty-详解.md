## NIO基础

> nio是一种**IO模型**，可以理解为 non-blocking io，非阻塞IO，Netty的核心。



### 1、三大组件

#### 1.1、Channel & Buffer

> Channel即**信道**，可以理解为一种通道，类比 Java 中的管道流，但Channel可以**双向**流通数据，Buffer即**缓冲**，客户端读取或输出数据流都需要先将数据流缓存到Buffer中，作为Channel的配套使用。
>

Channel是信道的**抽象父类**，其下常见的Channel有：

- FileChannel
- DatagramChannel（UDP）
- SocketChannel（TCP）
- ServerSocketChannel

Buffer也是缓冲的**抽象父类**，常见的Buffer有：

- ByteBuffer
  - MappedByteBuffer
  - DirectByteBuffer
  - HeapByteBuffer
- ShortBuffer
- IntBuffer
- LongBuffer
- FloatBuffer
- DoubleBuffer
- CharBuffer



#### 1.2、Selector

> Selector理解为选择器，作用偏向于中介，介于线程和信道之间
>

理解Selector的出现可以先理解传统上的并发请求处理流程。传统处理方式有**多线程版**、**线程池版**。

**多线程版**即给每个请求创建出一个线程处理Socket连接，但是具有如下缺点：

- 内存占用高
- 线程上下文切换成本高
- 只适合连接数少的场景

**线程池版**即多线程版使用线程池的改进版本，但也具有以下缺点：

- 处理上限仅为线程池的大小，且阻塞模式下，线程仅能处理一个socket连接
- 仅适合短连接场景

这些处理方法都是直接通过**处理线程**和**请求任务**进行沟通，类比于售房者直接与购房者沟通，效率较低，Selector的出现就类比于中介，提高了处理效率。

Selector配合一个线程管理多个Channel，获取这些Channel上发生的事件，这些Channel工作在非阻塞模式下，不会让线程阻塞在一个Channel上，而是哪个Channel需要线程就处理哪个。适合于连接数较多，但流量较低的场景。

![深入理解NioEventLoopGroup初始化- 赐我白日梦- 博客园](https://img2018.cnblogs.com/blog/1496926/201907/1496926-20190715230912442-1048567909.png)

调用Selector的 select() 方法会让Selector阻塞直到channel发生了读写就绪事件，当发生读写就绪事件后，select() 方法就会返回这些事件并交由 Thread 处理。



### 2、ByteBuffer

> 常用的文件缓冲区。
>

#### 2.1、正确使用姿势

```java
public static void main(String[] args) throws IOException {
    /**
         * ByteBuffer使用流程:
         *   1、向buffer中写入数据，例如调用channel.read(buffer)
         *   2、调用flip()切换模式为读模式
         *   3、从buffer读取数据，如调用buffer.get()
         *   4、调用clear() 或 compact() 切换至写模式
         *   5、重复1-4步骤
         */
    //通过文件输入流读取目标文件数据，并获得FileChannel对象
    FileChannel channel = new FileInputStream("data.txt").getChannel();
    //创建出大小为10的缓冲区
    ByteBuffer byteBuffer = ByteBuffer.allocate(10);
    while (true){
        //将信道中的数据读入缓冲区
        int read = channel.read(byteBuffer);
        if (read == -1)break;
        //将缓冲区改变为读模式（默认为写模式，写模式无法读取到数据）
        byteBuffer.flip();
        while (byteBuffer.hasRemaining()) {
            log.debug("{}", (char) byteBuffer.get());
        }
        //清除此缓冲区
        byteBuffer.clear();
    }
```



#### 2.2、ByteBuffer结构

ByteBuffer本质是个**byte数组**，其继承了Buffer抽象类，使用Buffer中的**三个变量**来控制自身数组结构：position、limit、capacity。

初始化时，position为0位置，limit和capacity为初始化容量位置。

写模式下，每向数组中写入一个元素，position就向后移一位。

flip动作下，改为读模式，position归0位置，limit至原position位置，读取范围为position - limit，每次get() position后移一位。

clear动作下，position归0位置，limit归capacity位置。

compact方法，将未读完的数据移至首位，position归未读完数据末尾，limit归至capacity，改为写模式。



#### 2.3、ByteBuffer常见方法





#### 2.4、Scattering Reads



#### 2.5、Gathering Writes



#### 2.6、练习



### 3、文件编程



### 4、网络编程



### 5、NIO vs BIO



