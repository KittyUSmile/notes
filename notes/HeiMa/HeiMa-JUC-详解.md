

## AQS原理

### 1.概述

AQS全称为AbstractQueuedSynchronizer，为阻塞式锁和相关的同步工具的框架。

特点：

- 用state属性来表示资源的状态（分独占模式和共享模式），子类需要定义如何维护这个状态，控制如何获取和释放锁。
  - getState - 获取state状态
  - setState - 设置state状态
  - compareAndSetState - cas 机制设置 state 状态
  - 独占模式是只有一个线程能够访问资源，而共享模式可以允许多个线程访问资源
- 提供了基于 FIFO 的等待队列，类似于 Moniter 的EntryList
- 条件变量来实现等待，唤醒机制，支持多个条件变量，类似于 Monitor 的WaitSet

子类主要实现这样一些方法（默认抛出 UnsupportedOperationException）

- tryAcquire
- tryRelease
- tryAcquireShared
- tryReleaseShared
- isHeloExclusively

获取锁的姿势：

```java
//如果获取锁失败
if(!tryAcquire(arg)){
    //入队，可以选择阻塞当前线程
}
```

释放锁的姿势：

```java
//如果释放锁成功
if(tryRelease(args)){
    //让阻塞线程恢复运行
}
```

