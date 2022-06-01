# 并发编程的挑战

## 上下文切换

对于单核CPU来说也可以支持多线程，CPU通过给每个线程分配时间片也实现这个机制。当一个线程执行完一个时间片后，CPU会进行线程切换，在切换前会保存当前线程状态，以便于下次切换回这个线程时，线程可以继续执行。

多线程不一定比单线程快，因为有线程切换等原因。

- 使用Lmbench3工具可以测量上下文切换的时长。
- 使用vmstat测量上下文切换的次数（Content Switch，CS表示上下文切换次数）。

### 如何减少上下文切换

主要方法有无锁并发编程、CAS算法、使用最少线程和使用协程。

- 无锁并发编程。多线程竞争锁时，会引起上下文切换，所以多线程处理数据时，可 以用一些办法来避免使用锁，如将数据的 ID 按照 Hash 算法取模分段，不同的线程 处理不同段的数据。
- CAS 算法。Java 的 Atomic 包使用 CAS 算法来更新数据，而不需要加锁。 
- 使用最少线程。避免创建不需要的线程，比如任务很少，但是创建了很多线程来处 理，这样会造成大量线程都处于等待状态。 
- 协程：在单线程里实现多任务的调度，并在单线程里维持多个任务间的切换。

### 减少上下文切换实战

通过减少WAITING的线程，等待的线程数少了，上下文切换次数也就少了。

## 死锁

```java
public class Main {
    private static String A = "A";
    private static String B = "B";
    public static void main(String[] args) {
        new Main().deadLock();
    }
    private void deadLock() {
        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (A) {
                    try {
                        Thread.sleep(2000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (B) {
                        System.out.println("1");
                    }
                }
            }
        });
        Thread t2 = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (B) {
                    synchronized (A) {
                        System.out.println("2");
                    }
                }
            }
        });
        t1.start();
        t2.start();
    }
}
```

避免死锁的几个常见方法：

- 避免一个线程同时获取多个锁。
- 避免一个线程在锁内同时占用多个资源，尽量保证每个锁只占用一个资源。
- 尝试使用定时锁，使用 lock.tryLock（timeout）来替代使用内部锁机制。
- 对于数据库锁，加锁和解锁必须在一个数据库连接里，否则会出现解锁失败的情况。

## 资源限制的挑战

### 什么是资源限制

硬件资源限制有带宽、硬盘读写速度、CPU速度，软件资源限制有数据库连接数和socket连接数。并发编程时要考虑这些因素。

### 资源限制引发的问题

多线程造成资源瓶颈时，多线程相比单线程不会加快程序运行速度。

### 如何解决资源限制的问题

根据不同的资源限制调整程序的并发度。

# Java并发机制的底层实现原理

## volatile的应用

volatile是轻量级的synchronized，它在多处理器开发中保证了共享变量的“可见性”。可见性的意思是当一个线程修改一个共享变量时，另外一个线程能读到这个修改的值。如果使用得当volatile比synchronized成本低，因为它不会引起线程上下文切换和调度。

### volatile的定义与实现原理

Java 语言规范第 3 版中对 volatile 的定义如下：Java 编程语言允许线程访问共享变 量，为了确保共享变量能被准确和一致地更新，线程应该确保通过排他锁单独获得这个变量。

<div align='center'>
    <img src='./imgs/JavaPA/001.png' width='800px'>
    <br/><br/>CPU术语定义
</div>

有volatile变量修饰的共享变量进行写操作的时候在汇编层会有一个Lock指令。Lock前缀的指令在多核处理器下会引发两件事情。

- 将当前处理器缓存行的数据写回到系统内存。
- 写会操作会导致其他CPU里缓存了该内存地址的数据无效。

volatile实现原则：

1. ##### **==Lock前缀指令会引起处理器缓存写会内存。==**

   Lock信号会锁缓存，一般不会锁总线（开销太大）。

2. **==一个处理器的缓存写回到内存会导致其他处理器的缓存无效。==**

   IA-32和Intel 64处理器能嗅探其他处理器访问系统内存和它们的内部缓存。处理器使用嗅探技术保证它的内部缓存、系统内存和其他处理器的缓存的数据在总线上保持一致。如果通过嗅探一个处理器来检测其他处理器打算写内存地址，而这个地址当前处于共享状态，那么正在嗅探的处理器将使它的缓存行无效，在下次访问相同内存地址时，强制执行缓存行填充。

### volatile的使用优化

JDK7的并发包新增LinkedTransferQueue，会将队头、队尾节点通过追加字节方式追加到64B（一个缓存行），这样会保证队头、队尾节点不在同一缓存行，当修改其中一个的时候不会锁住另一个。

```java
/**
* 队列中的头部节点
*/
private transient final PaddedAtomicReference<QNode> head;
/**
* 队列中的尾部节点
*/
private transient final PaddedAtomicReference<QNode> tail;
static final class PaddedAtomicReference<T> extends AtomicReference T>
{
 // 使用很多 4 个字节的引用追加到 64 个字节
 Object p0, p1, p2, p3, p4, p5, p6, p7, p8, p9, pa, pb, pc, pd, pe;
 PaddedAtomicReference(T r) {
 super(r);
}
}
public class AtomicReference<V> implements java.io.Serializable {
 private volatile V value;
 // 省略其他代码
}
```



并不是所有的volatile变量都应该增加到64B，在两种场景下都不应该使用追加字节的方式。

1. 缓存行非64B宽的处理器。
2. 共享变量不会被频繁地写。

这种追加字节的方式在java7中可能不生效，它会淘汰或重新排序无用字段，需要使用其他追加字节的方式。

## synchronized的实现原理与应用

Java中的每一个对象都可以作为锁。具体表现为以下3中形式：

1. 对于普通同步方法，锁是当前实例对象。
2. 对于静态同步方法，锁是当前类的Class对象。
3. 对于同步方法块，锁是Synchronized括号里配置的对象。

JVM基于进入和退出Monitor对象来实现方法和代码块同步，但两者实现细节不同。代码块同步是使用monitorenter和monitorexit指令实现的，而方法同步是使用另外一种方式实现的。

### Java对象头

synchronized用的锁是存在Java对象头里的。如果对象是数组类型，则虚拟机用 3 个字宽（Word）存储对象头，如果对象是非数组类型，则用 2 字宽存储对象头。

<div align='center'>
    <img src='./imgs/JavaPA/002.png' width='800px'>
    <br/><br/>Java对象头的长度
</div>

Java 对象头里的 Mark Word 里默认存储对象的 HashCode、分代年龄和锁标记位。32 位 JVM 的 Mark Word 的默认存储结构如下。

<div align='center'>
    <img src='./imgs/JavaPA/003.png' width='800px'>
    <br/><br/>Java对象头的存储结构
</div>

在运行期间，Mark Word 里存储的数据会随着锁标志位的变化而变化。

<div align='center'>
    <img src='./imgs/JavaPA/004.png' width='800px'>
    <br/><br/>Mark Word的状态变化
    <img src='./imgs/JavaPA/005.png' width='800px'>
    <br/><br/>Mark Word的存储结构（64bit）
</div>

### 锁的升级与对比

锁一共有4中状态，级别从低到高依次是：无锁状态、偏向锁状态、轻量级锁状态和重量级锁状态（随竞争情况逐渐升级）锁可以升级但不能降级，目的是为了提高获得锁和释放锁的效率。

#### 偏向锁

大多数情况下，锁不仅不存在多线程竞争，且总是由同一线程多次获得，引入偏向锁。当一个线程访 问同步块并获取锁时，会在对象头和栈帧中的锁记录里存储锁偏向的线程 ID，以后该线 程在进入和退出同步块时不需要进行 CAS 操作来加锁和解锁，只需简单地测试一下对象头的 Mark Word 里是否存储着指向当前线程的偏向锁。如果测试成功，表示线程已经获得了锁。如果测试失败，则需要再测试一下 Mark Word 中偏向锁的标识是否设置成 1 （表示当前是偏向锁）：如果没有设置，则使用 CAS 竞争锁；如果设置了，则尝试使用 CAS 将对象头的偏向锁指向当前线程。

- 偏向锁的获得和撤销

	<div align='center'>
	    <img src='./imgs/JavaPA/006.png' width='800px'>
	    <br/><br/>偏向锁的获得和撤销
	</div>

- 关闭偏向锁

	偏向锁在 Java 6 和 Java 7 里是默认启用的，但是它在应用程序启动几秒钟之后才激 活，如有必要可以使用 JVM 参数来关闭延迟：-XX:BiasedLockingStartupDelay=0。如果 你确定应用程序里所有的锁通常情况下处于竞争状态，可以通过 JVM 参数关闭偏向锁： -XX:- UseBiasedLocking=false，那么程序默认会进入轻量级锁状态。

#### 轻量级锁

<div align='center'>
    <img src='./imgs/JavaPA/007.png' width='800px'>
    <br/><br/>轻量级锁及膨胀流程图
</div>

- 轻量级锁加锁

	线程在执行同步块之前，JVM 会先在当前线程的栈桢中创建用于存储锁记录的空 间，并将对象头中的 Mark Word 复制到锁记录中，官方称为 Displaced Mark Word。然后 线程尝试使用 CAS 将对象头中的 Mark Word 替换为指向锁记录的指针。如果成功，当前线程获得锁，如果失败，表示其他线程竞争锁，当前线程便尝试使用自旋来获取锁。

- 轻量级锁解锁

	轻量级解锁时，会使用原子的 CAS 操作将 Displaced Mark Word替换回到对象头， 如果成功，则表示没有竞争发生。如果失败，表示当前锁存在竞争，锁就会膨胀成重量级锁。

	因为自旋会消耗 CPU，为了避免无用的自旋（比如获得锁的线程被阻塞住了），一 旦锁升级成重量级锁，就不会再恢复到轻量级锁状态。当锁处于这个状态下，其他线程 试图获取锁时，都会被阻塞住，当持有锁的线程释放锁之后会唤醒这些线程，被唤醒的 线程就会进行新一轮的夺锁之争。

- 锁的优缺点

	<div align='center'>
	    <img src='./imgs/JavaPA/008.png' width='800px'>
	    <br/><br/>锁的优缺点对比
	</div>

## 原子操作的实现原理

原子操作（atomic operation）：不可被中断的一个或一系列操作。

<div align='center'>
    <img src='./imgs/JavaPA/009.png' width='800px'>
    <br/><br/>相关术语
</div>

### 处理器如何实现原子操作

处理器会自动保证基本的内存操作（读取或写入一个字节）的原子性。处理器提供总线锁定和缓存锁定两个机制来保证复杂内存操作的原子性。

#### 使用总线锁保证原子性

LOCK#信号，当一个处理器在总线上输出此信号时，其他处理器的请求将被阻塞住，那么该处理器可以独占共享内存。

#### 使用缓存锁保证原子性

总线锁开销大（锁期间CPU不能与内存通信）。如果锁住的数据在缓存中，会将数据写回主存，通过缓存一致性来保证原子性。

但是会有两种情况下处理器不会使用缓存锁定：

1. 操作的数据不能被缓存到处理器缓存中，或操作的数据跨多个缓存行，处理器会调用总线锁定。
2. 有些处理器不支持缓存锁定，就算要锁定的内存区域在缓存行中也会调用总线锁定。

针对上述两种情况，Intel处理器提供了许多Lock前缀的指令，这些指令操作的内存区域会加锁，导致其他处理器不能同时访问它。

### JAVA如何实现原子操作

通过锁和循环CAS的方式实现。

#### 使用循环CAS实现原子操作

JVM中的CAS 操作正是利用了处理器提供的CMPXCHG指令实现的。自旋 CAS 实现的基本思路就是循环进行 CAS 操作直到成功为止。

JDK并发包里提供了一些类来支持原子类操作。

```java
public class Test {
    private AtomicInteger atomicI = new AtomicInteger(0);
    private int i = 0;
    public static void main(String[] args) {
        final Test cas = new Test();
        List<Thread> ts = new ArrayList<Thread>(600);
        long start = System.currentTimeMillis();
        for (int j = 0; j < 100; j++) {
            Thread t = new Thread(new Runnable() {
                @Override
                public void run() {
                    for (int i = 0; i < 10000; i++) {
                        cas.count();
                        cas.safeCount();
                    }
                }
            });
            ts.add(t);
        }
        for (Thread t : ts) {
            t.start();
        }
        // 等待所有线程执行完成
        for (Thread t : ts) {
            try {
                t.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println(cas.i);
        System.out.println(cas.atomicI.get());
        System.out.println(System.currentTimeMillis() - start);
    }
    /**
     * 使用 CAS 实现线程安全计数器
     */
    private void safeCount() {
        for (; ; ) {
            int i = atomicI.get();
            boolean suc = atomicI.compareAndSet(i, ++i);
            if (suc) {
                break;
            }
        }
    }
    /**
     * 非线程安全计数器
     */
    private void count() {
        i++;
    }
}
```

#### CAS实现原子操作的三大问题

1. ==ABA问题。==CAS通过比较值有没有变化来实现原子操作，但是如果一个值原来是A，之后变成了B，最后又变回了A，那么使用CAS检查的时候发现它的值并没有发生变化，但实际上却发生了变化。解决思路就是使用版本号。

	```java
	public boolean compareAndSet(
	 V expectedReference, // 预期引用
	 V newReference, // 更新后的引用
	 int expectedStamp, // 预期标志
	 int newStamp // 更新后的标志
	)
	```

2. ==循环时间长开销大。==CAS长时间不成功，会给CPU带来非常大的执行开销。如果 JVM 能支持处理器提供的 pause 指令，那么效率会有一定的提升。pause 指令 有两个作用：第一，它可以延迟流水线执行指令（de-pipeline），使 CPU不会消耗过多的执行资源，延迟的时间取决于具体实现的版本，在一些处理器上延迟时间是零；第二，它可以避免在退出循环的时候因内存顺序冲突（Memory Order Violation）而引起 CPU流水线被清空（CPU Pipeline Flush），从而提高 CPU 的执行效率。

3. ==只能保证一个共享变量的原子操作。==当对一个共享变量执行操作时，我们可以使用 循环 CAS 的方式来保证原子操作，但是对多个共享变量操作时，循环 CAS 就无法保证操作的原子性，这个时候就可以用锁。还有一个取巧的办法，就是把多个共享变量合并 成一个共享变量来操作。

#### 使用锁机制实现原子操作

锁机制保证了只有获得锁的线程才能够操作锁定的内存区域。JVM 内部实现了很多 种锁机制，有偏向锁、轻量级锁和互斥锁。有意思的是除了偏向锁，JVM 实现锁的方式 都用了循环 CAS，即当一个线程想进入同步块的时候使用循环 CAS 的方式来获取锁， 当它退出同步块的时候使用循环CAS释放锁。

# Java内存模型

## Java内存模型的基础

### 并发编程模型的两个关键问题

线程之间如何通信以及线程之间如何同步。

在共享内存的并发模型里，线程之间共享程序的公共状态，通过写-读内存中的公共 状态进行隐式通信。在消息传递的并发模型里，线程之间没有公共状态，线程之间必须通过发送消息来显式进行通信。

同步是指程序中用于控制不同线程间操作发生相对顺序的机制。在共享内存并发模 型里，同步是显式进行的。程序员必须显式指定某个方法或某段代码需要在线程之间互 斥执行。在消息传递的并发模型里，由于消息的发送必须在消息的接收之前，因此同步 是隐式进行的。

Java 的并发采用的是共享内存模型，Java 线程之间的通信总是隐式进行，整个通信 过程对程序员完全透明。

### Java内存模型的抽象结构

