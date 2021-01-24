## JUC（java.util.concurrent）

1.概念

- 进程：一个程序
- 线程：线程是操作系统能够进行运算调度的最小单位,线程是进程的子集，一个进程可以有很多线程，每条线程并行执行不同的任务,不同的进程使用不同的内存空间，而所有的线程共享一片相同的内存空间。别把它和栈内存搞混，每个线程都拥有单独的栈内存用来存储本地数据。

2.

- 并发：
- 并行：

3.包结构

- java.util.concurrent
- java.util.concurrent.atomic
- java.util.concurrent.locks

4.lock

- 比synchronize方式更好
- 在try外面上锁，在finally里释放锁
- 实现类
  - ReentrantLock：可重入锁 

5.线程start是否立即启动

否，等待线程调度

6.线程状态

- new
- runnable
- blocked
- waiting
- timed_waiting
- terminated

7.线程不安全前提

- 多个线程

8.解决ArrayList并发问题方式

- Vector
- Collections.synchronizedList(new ArrayList())
- CopyOnWriteArrayList
  - 写时复制：CopyOnWrite容器即写时复制容器，往一个容器里添加元素时，不直接往当前容器Object[]添加，而是先将当前容器Object[]进行copy，夫指出一个新的容器Object[] newElements，然后新的容器Object[] newElements里添加元素，添加完成后，再将原容器的引用指向新的容器setArray(newElements)；这样做的好处是可以对CopyOnWrite容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以CopyONWrite容器也是一种读写分离的思想，读和写不同的容器 
- ArrayList --> CopyOnWriteArrayList
- HashSet --> CopyOnWriteArraySet
- HashMap --> CurrentHashMap

9.异常

- java.util.ConcurrentModificationException

10.分类

- 对象锁
- 类锁

11.虚假唤醒

- while替代if

12.技术替换

- synchronize   ---   lock
- wait   ---   condition.await
- notify   ---   condition.signal
- notifyAll   ---   condition.signalAll 
- 替换目的：可以准确调用特定线程，原始synchronize实现需要借助线程优先级实现

13.FutureTask

- 实现了runnable接口

14.CountDownLatch

- 线程计数器：用于等待特定数目的线程都执行完再开始执行

15.CyclicBarrier

- 与CountDownLatch类似，区别是可以重复使用

16.Semaphore

- 抢占多资源

17.读写锁

- ReadWriteLock 

18.BlockingQueue

- 当队列是空的，从队列获取元素的操作将会被阻塞
- 当队列是满的，从队列添加元素的操作将会被阻塞

![image-20201106153211081](.\image-20201106153211081.png)

生产者消费者模式，正常生产环境不用wait - notify 实现，而是直接使用阻塞队列

![image-20201106153746728](.\image-20201106153746728.png)

满了add 抛异常：IllegalStateException：Queue full

不满remove 抛异常：NoSuchElementException

满了offer：返回false

不满poll：返回null

阻塞：put / take

计时等待：offer(e, time, unit)

![image-20201106181429092](.\image-20201106181429092.png)

19.读写锁

ReentrantReadWriteLock

- Lock readLock
- Lock writeLock

支持锁降级：不释放写锁前提下申请读锁，写锁降为读锁

不支持锁升级：不释放读锁前提下申请写锁，导致死锁

20.volatile

- 如果你使用锁来保护可以被多个线程访问的代码，那么可以不考虑这种问题。编译器被要求通过在必要的时候刷新本地缓存来保持锁的效应。

- 不保证原子性

- 假设对共享变量除了赋值之外并不完成其他操作，那么可以将这些共享变量声明为volatile。

- java.util.concurrent.atomic包中有很多类使用了很搞笑的机器指令（而不是使用锁）来保证其他操作的原子性。

  - 有些操作，例如：根据最新的值更新，需要循环判断

    - ```java
      do {
      	oldValue = largest.get();
      	newValue = Math.max(oldValue, observed);
      } while (!largest.compareAndSet(oldValue, newValue));
      ```

      java8中可以用lambda表达式操作：largest.updateAndGet(x -> Math.max(x, observed));

21.线程局部变量

ThreadLocal
