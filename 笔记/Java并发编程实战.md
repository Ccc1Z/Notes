# Java并发编程实战

## 第一章：简介

- 并发能够使复杂的**异步代码**变得更简单

### 1.1 并发简史

- 随着操作系统的出现，计算机每次能够运行多个程序，并且**每个程序都在单独的进程中运行**
  - 操作系统为各个独立执行的进程分配各种资源：
    - 内存，文件句柄以及安全证书等
- 不同的进程之间可以通过一些**粗粒度的通信机制**来交换数据
  
- 套接字，信号处理器，共享内存，信号量以及文件等
  
- 三个采用多进程的原因
  - 资源利用率
  - 公平性
  - 便利性

- 以上这些促使进程出现的因素同样也促使着线程的出现

  > 线程允许在同一个进程中同时存在多个程序控制流
  >
  > 线程会共享进程范围内的资源
  >
  > ​		★内存句柄
  >
  > ​		★文件句柄
  >
  > 但每个线程都有各自的```程序计数器(Program Counter)、栈以及局部变量等```
  - 线程还提供了一种直观的分解模式来充分利用多处理器系统中的硬件并行性
  - 而在**同一个程序中的多个线程也可以被同时调度到多个CPU上运行**

- 线程也被称为轻量级进程。在大多数现代操作系统中，都是**以线程为基本的调度单位，而不是进程。**

**如果没有明确的协同机制，那么线程将被彼此独立执行。**

> 同一个进程内的所有线程都会共享进程的内存地址空间

- 因此这些线程都能访问相同的变量并且在**同一个堆上分配对象**
  - 这就需要实现一种比在进程间共享数据粒度更细的数据共享机制
  - 如果没有明确的`同步机制`来协同对共享数据的访问，`那么当一个线程正在使用某个变量时，另一个线程可能同时访问这个变量`，这将造成不可预测的结果。

### 1.2 线程的优势

- 线程能够将大部分的`异步工作流`转换成`串行工作流`
- 发挥多处理器的强大能力
- 建模的简单性
- 异步事件的简化处理
  - 如果某个应用程序对套接字执行读操作而此时还没有数据到来，**那么这个读操作将一直阻塞直到有数据到达。**
  - 在单线程应用程序中，这不仅意味着在处理请求的过程中将停顿，**而且还意味着在这个线程被阻塞期间，对所有请求的处理都将停顿。**
  - 为了避免这个问题，单线程服务器应用程序必须使用非阻塞I/O(NIO是一种java1.4引入的非阻塞I/O模型)

### 1.3 线程带来的风险

#### 1.3.1 安全性问题

- 线程安全性可能是非常复杂的，`在没有充足同步的情况下，多个线程中的操作执行顺序是不可预测的。`

```java
@NotThreadSafe
public class UnsafeSequence{
    private int value;
    
    /**返回一个唯一的数值。*/
    public int getNext(){
        return value++;
    }
}
```

- `value++`包含了三个`独立`的操作
  - `读取`value
  - 将value`加1`
  - 将计算结果`写入`value

![](D:\MultiProcessAndConcurrentBook\imgs\图1.1.png)

**线程安全的核心问题**

> 因为多个线程要`共享相同的内存地址空间`并且是`并发运行`
>
> 因此它们可能会`访问或修改其他线程正在使用的变量`

**要使多线程程序的行为可以预测，必须对`共享变量`的访问操作进行`协同操作`，这样才不会`在线程之间发生彼此干扰`**

```java
package chapter01;
@ThreadSafe
public class Sequence {
    private int value;
    
    public synchronized int getNext(){
        return value++;
    }
}
```

#### 1.3.2 活跃性问题

- `安全性`：永远不发生糟糕的事情
- `活跃性`：某件正确的事情最终会发生
  - 当某个操作`无法继续执行下去`时，就会发生活跃性问题。
- `活跃性问题的形式之一`就是`死循环`
  - 如果`线程A`在`等待线程B释放其持有资源`，但`线程B永远不会释放其资源`
  - 这就会造成`线程A`一直在等待

#### 1.3.3 性能问题

- `性能问题与活跃性问题密切相关`
- 性能问题包含多个方面

> 服务时间过长
>
> 响应不灵敏
>
> 吞吐率过低
>
> 资源消耗过高
>
> 可伸缩性较低等

**在多线程程序中，当线程调度器`临时挂起`一个`活跃线程`并转而`运行另一个线程`时，就会频繁地出现`上下文切换操作(Context Switch)`，这种操作会带来极大的`开销`:**

> `保存`和`恢复`上下文
>
> `丢失局部性`
>
> 并且CPU时间将更多地花在`线程调度`而不是`线程运行`上

**当线程共享数据时，必须使用`同步机制`，这些机制往往会抑制某些编译器优化，使`内存缓存区中的数据`无效，以及增加`共享内存总线的同步流量`。



## 第二章：线程安全性

- 正确地使用`线程`和`锁`只是一些机制，线程安全的**核心在于要对`状态访问操作进行管理`，特别是对`共享的(Shared)`和`可变的(Mutable)`状态的访问**

- 对象的状态：

> 对象的状态是指`存储在状态变量`（例如实例或静态域）中的数据
>
> 对象的状态可能包括`其他依赖对象的域`
>
> 例如，某个`HashMap`的状态不仅存储在`HashMap`对象本身
>
> 还存储在许多`Map.Entry`对象中
>
> 在对象的状态中包含了任何可能影响其`外部可见行为`的数据

**java中的同步机制：**

- 关键字`synchronized`，它提供了一种`独占的加锁方式`
- `volatitle`类型的变量
- `显式锁(Explicit Lock)`
- `原子变量`

**如果当多个线程访问`同一个可变的状态变量`时没有使用`合适的同步`，三种方式修复**

- 不在线程之间共享该状态变量
- 将状态变量修改为不可变的变量
- 在访问状态变量时使用同步

**几个知识点**

> 1. 线程安全的`程序`不是完全由线程安全`类`构成的
> 2. 完全由线程安全`类`构成的`程序`不一定时`线程安全的
> 3. `线程安全类`中也可以包含`非线程安全的类`
> 4. 在任何情况中，**只有当`类`中仅包含`自己的状态时`，`线程安全类才有意义`**



### 2.1 什么是线程安全

**线程安全性的定义中，最核心的概念就是`正确性`**

- 正确性的含义是：某个类的`行为`与其`规范`完全一致
  - 在良好的规范中通常会定义`各种不变性条件(Invariant)`来约束对象的状态
  - 以及定义各种`后验条件（Postcondition）`来描述对象操作的结果

**当`多个线程`访问某个类时，`这个类`始终都表现出`正确的行为`，那么就称`这个类是线程安全的`**

**`无状态对象`一定是线程安全的**



### 2.2原子性

#### 2.2.1 竞态条件

- 当某个计算的正确性取决于`多个线程的交替执行时序时`，那么就会发生`竞态条件`

**本质：基于一种`可能失效`的观察结果来做出判断或者执行某个计算**

- 这种类型的`竞态条件`称为`先检查后执行`
  - 首先观察到某个条件`为真`（例如文件X不存在）
  - 然后根据这个观察结果采用相应的动作（创建文件X）
  - 但事实上，在你`观察到这个结果`以及`开始创建文件之间`，**观察结果可能变得无效**（另一个线程在这期间创建了文件X）

#### 2.2.2示例：延迟初始化中的竞态条件

- `延迟初始化`的目的是将对象的`初始化操作`推迟到实际被使用时才进行
- 同时要确保`只被初始化一次`

```java
package chapter01;

@NotThreadSafe
public class LazyInitRace {
    private Object instance = null;
    
    public Object getInstance(){
        if(instance == null){
            instance = new Object();
        }
        return instance;
    }
}
```

#### 2.2.3 复合操作

**要避免竞态条件问题，就必须在`某个线程修改该变量时`，通过`某种方式`防止`其他线程`使用这个变量**

- 从而确保其他线程只能在`修改操作`完成之前或之后`读取`和`修改`状态，而不是在修改状态的过程中

> 假设有两个操作`A`和`B`
>
> 如果从执行`A`的线程来看，当`另一个线程`执行`B`时
>
> 要么将`B全部执行完`，要么`完全不执行B`
>
> 那么`A`和`B`对彼此来说时**`原子的`**

**原子操作是指：**

- 对于访问`同一个状态`的所有操作（包括该操作本身）来说，这个操作是一个以`原子方式`执行的操作。



### 2.3 加锁机制

#### 2.3.1 内置锁

**Java提供了一种内置的锁机制来支持原子性：`同步代码块(Synchronized Block)`**：

- 同步代码块包括两个部分：
  - 一个作为`锁的对象引用`
  - 一个作为由这个锁保护的`代码块`

```java
synchronized(lock){
    //访问或修改由锁保护的共享状态
}
```

**Java的内置锁相当于是`重型锁(互斥锁)`,虽然保证了线程安全性，但严重影响了性能**

#### 2.3.2 重入

**内置锁是`可重入`的**

> 如果`某个线程`试图获得一个已经由它自己持有的锁，那么这个请求就会成功
>
> `重入`意味着`获取锁`的操作的粒度是`线程`，而不是`调用`

**重入的实现方法**

- 为每个`锁`关联一个`获取计数值`和一个`所有者线程`
- 当`计数值`为0时，这个锁就被认为时没有被任何线程持有
- 当线程请求一个`未被持有的锁时`，`JVM`会记下锁的持有者
- 并且将`获取计数值置为1`
- 如果`同一个线程再次获取这个锁`，`计数值将递增`
- 而当`线程退出同步代码块时`，计数器会相应的`递减
- 当计数值为0时，这个锁将被`释放`

**重入避免了访问同一个`锁对象`可能发生的`死锁`情况**



### 2.4 用锁来保护状态

### 2.5 性与性能

## 第三章： 对象的共享

- 第2章介绍了如何通过`同步`来避免多个线程在`同一时刻`访问相同的数据
- 本章将介绍如何`共享和发布对象`，从而使它们能够安全地由多个线程`同时访问`

**这两章合在一起，就形成可`构建线程安全类`以及通过`java.util.concurrent`类库(JUC)来构建并发应用程序的重要基础**

**`Synchronized`不仅能够用于实现`原子性`，同步还能实现`内存可见性(Memory Visibility)`**

### 3.1 可见性

- 在`单线程环境`种，如果向某个变量`先写入值`，然后在没有其他写入操作的情况下`读取这个变量`，那么总能得到`相同的值`
- 然而，当`读操作`和`写操作`在`不同的线程中`执行时，情况并非如此

```java
package chapter03;

public class Novisibility {
    private static boolean ready;
    private static int number;

    public static class ReaderTherd implements Runnable{
        @Override
        public void run(){
            while(!ready){
                Thread.yield();
            }
            System.out.println(number);
        }
    }

    public static void main(String[] args) {
        Thread thread1 = new Thread(new ReaderTherd());
        thread1.start();
        number = 42;
        ready = true;

    }
}
```

- 上面的程序，虽然Novisibility看起来会输出42，但事实上很可能输出0，或者根本无法终止。
  - 无限循环：线程可能永远都看不到`ready`的值
  - 输出0：**发生了指令重排序**,读线程可能看到了写入`ready`的值，但却没有看到之后`写入number`的值
    - 先执行了`ready = true`

- **只要由数据在多个线程之间`共享`，就使用正确的`同步`**

#### 3.1.1 失效数据

- `NoVisibility`展示了在缺乏`同步`的程序中可能产生错误结果的一种情况：`失效数据`
  - 当读线程查看`ready`变量时，可能会得到一个`已经失效的值`

```java
package chapter03;

@NotThreadSafe
public class MultableInteger {
    private int value;

    public int getValue() {
        return value;
    }

    public void setValue(int value) {
        this.value = value;
    }
}
```

- `get()`和`set()`都是在没有同步的情况下访问value的
  - 如果某个线程调用了`set`，那么另一个正在调用`get`的线程可能会看到更新后的value值，也可能看不到

```java
package chapter03;
@ThreadSafet
public class SynchronizedInteger {
  @GuardedBy("this")  private int value;
    public synchronized int get(){return value;}
    public synchronized void set(int value){this.value = value;}
}
```

#### 3.1.2 非原子的64位操作

**最低安全性**：

- 当线程在`没有同步`的情况下读取变量时，可能得到一个失效值
- 但这个失效值时由之前某个线程设置的值，`不是一个随机值`
- 这种安全性保证也被称为`最低安全性`

**对于非`volatile`类型的`64位数值变量(double和long)`**

- `Java内存模型`要求，变量的`读取操作`和`写入操作`都必须时`原子操作`
- 但对于非`volatile`类型的`long`和`double`变量，`JVM`允许将`64位`的`读操作`或`写操作`分解为`两个32位的操作`
  - 所以，在不同的线程中对该变量执行`读操作`和`写操作`可能会得到某个值的`高32位`和另一个值得`低32位`

#### 3.1.3 加锁与可见性

**加锁得含义不仅仅局限于`互斥行为`，还包括`内存可见性`。为了确保所有线程都能看到`共享变量`的`最新值`，所有执行`读操作`或`写操作`的线程都必须在同一个锁上同步



#### 3.1.4 Vloatile变量

**Java提供了一种`稍弱的同步机制`，即`volatile`变量，来确保将变量的更新操作通知到其他线程**

**当把变量声明位`volatile`类型后，编译器与运行时都会注意到这个变量是`共享`的，因此不会讲该变量上的操作与其他内存操作一起`重排序`**

- volatile的两个特性

  > 线程可见性
  >
  > 阻止指令重排序

- volatile变量不会不`缓存在寄存器`或者对`其他处理器不可见的地方`，因此在读取`volatile`类型的变量时总会返回最新写入的值

**`volatile`的局限性：**

> `volatile变量`通常用做某个操作完成、发生或重锻或者状态的`标志`
>
> 但是`volatile`不具备`原子性`

**加锁机制既可以保证`可见性`和`原子性`，但`volatile变量`只能确保`可见性`。**

**当且仅当满足以下`所有条件`时，才应该使用`volatile变量`**

> - 对变量的写入操作`不依赖`变量的当前值(i++就不行)，或者你能确保只有单个线程更新变量的值
> - 该变量不会与其他状态变量一起纳入不变性条件中
> - 在访问变量时不需要加锁



### 3.2 发布于逸出

- `发布(publish)`一个对象是指，使对象能够在`当前作用域之外`的代码中使用。
  - 例如，讲一个指向该对象的引用保存到其他代码可以访问的地方，
  - 或者在某一个非私有方法中返回该引用
  - 或者讲引用传递到其他类的方法中

- 当某个不该发布的对象被发布时，这种情况就被称为`逸出(Escape)`

```java
package chapter03;

import java.util.HashSet;
import java.util.Set;

/**
 * 发布一个对象
 */
public class ObjcetPublish {
    public static Set<Object> knownSecrets;

    public void initialize(){
        knownSecrets = new HashSet<Object>();
    }
}
```

- 在发布`knownSectets`对象时，会同时发布`Object`对象

```java
package chapter03;

/**
 * 逸出
 */
public class UnsafeStates {
    private String[] states = new String[] {
            "AK","AL"
    };

    public String[] getStates() {
        return states;
    }
}

```

- 发布`UnsafeStates`时会同时发布本应为`私有的状态数组`
  - 示例中`states数组`已经逸出了它所在的作用域，这个本应是`私有的变量`已经被发布了

**安全的对象构造过程**

- 如果`this引用`在`构造过程中`逸出，那么这种对象就被认为时不正确构造

- 在`构造函数`中`启动一个线程`会造成`this引用`逸出
- 在`构造函数`中调用一个`可改写的实例方法时`(既不是私有方法，也不是中介方法)，同样会导致`this引用`在构造过程中逸出
- 使用`工厂方法`来防止`this引用`在`构造过程中`逸出

```java
package chapter03;

import jdk.internal.event.Event;

import java.util.EventListener;

/**
 * 使用工厂方法来防止this引用在构造过程中逸出
 */
public class SafeListener {
    private final EventListener listener;

    private SafeListener(){
        listener = new EventListener() {
            public void onEvent(Event e){
                //do something
            }
        };
    }

    public static SafeListener newInstance(EventSource source){
        SafeListener safe = new SafeListener();
        source.registerListener(safe.listener);
        return safe;
    }
}

```

### 3.3 线程封闭

- `线程封闭`就是不共享数据，仅在单线程内访问数据

- 常在`JDBC`中使用，隐含的将`Connection对象`封闭在线程中
  -  服务器提供的`连接池`是线程安全的
  - `Connection对象`不要求是线程安全的
- `局部变量`和`ThreadLocal`类能够帮助维持线程封闭性，但是还是需要负责确保封闭在线程中的对象不会从线程中`逸出`

#### 3.3.1 Ad-hoc 线程封闭(少用)

- `Ad-hoc线程封闭`是指，维护线程封闭性的职责完全由`程序实现来承担`

- `Ad-hoc`是非常脆弱的

#### 3.3.2 栈封闭

- 在`栈封闭`中，只能通过`局部变量`才能访问对象
  - **`局部变量`的固有属性之一就是`封闭在执行线程`中**
  - 它们位于执行线程的`栈`中，其他线程无法访问

#### 3.3.3  ★ThreadLocal类★

- ThreadLocal 线程本地变量
- ThreadLocal是一个对象(可以想象成一个容器，一个容器里面可以装对象)
- 绑定在每一个不同线程上的对象---线程本地对象**(每个线程独立拥有线程存在 ThreadLocal就一直存在)**
- **tl.set(new Person())相当于是将Person装进了自己线程的一个map里面了**

```java
package cn.cqu.ccc1z.ThreadLocal;

import java.util.concurrent.TimeUnit;

public class ThreadLocal1 {
    //volatile static Person p = new Person();
    //ThreadLocal是一个对象
    //把ThreadLocal看作一个容器，容器里面又装了一个Person对象
    static ThreadLocal<Person> tl = new ThreadLocal();

    public static void main(String[] args) {

        //2秒以后去读tl里面装的对象
        //它读不到，因为ThreadLocal是线程本地的一个对象
        //ThreadLocal一定只与它自己的那个线程有关
        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(tl.get());
        }).start();

        //1秒以后往ThreadLocal里装一个Person对象
        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //又装了一个Person对象
            tl.set(new Person());
        }).start();
    }

    static class Person{
        String name = "zhangsan";
    }
}
output:null
```

```java
public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        map.set(this, value);
    } else {
        createMap(t, value);
    }
}
//源码
```

```java
ThreadLocalMap getMap(Thread t) {
    return t.threadLocals;
}
//源码
```

```java
ThreadLocal.ThreadLocalMap threadLocals = null;
//源代码
```

- 先拿到当前的这个Thread对象
- 创建一个map
- **key:这个ThreadLocal对象**
- **value:是我们new出来的对象**
- map是当前线程对象里面的一个map

- 读到最后，threadLocals是ThreadLocal的一个成员变量

![](D:\MultiProcessAndConcurrent\弱引用.PNG)



- map.set()

```java
/**
 * Set the value associated with key.
 *
 * @param key the thread local object
 * @param value the value to be set
 */
private void set(ThreadLocal<?> key, Object value) {

    // We don't use a fast path as with get() because it is at
    // least as common to use set() to create new entries as
    // it is to replace existing ones, in which case, a fast
    // path would fail more often than not.

    Entry[] tab = table;
    int len = tab.length;
    int i = key.threadLocalHashCode & (len-1);

    for (Entry e = tab[i];
         e != null;
         e = tab[i = nextIndex(i, len)]) {
        ThreadLocal<?> k = e.get();

        if (k == key) {
            e.value = value;
            return;
        }

        if (k == null) {
            replaceStaleEntry(key, value, i);
            return;
        }
    }

    tab[i] = new Entry(key, value);
    int sz = ++size;
    if (!cleanSomeSlots(i, sz) && sz >= threshold)
        rehash();
}
```

- Entry

```java
/**
 * The entries in this hash map extend WeakReference, using
 * its main ref field as the key (which is always a
 * ThreadLocal object).  Note that null keys (i.e. entry.get()
 * == null) mean that the key is no longer referenced, so the
 * entry can be expunged from table.  Such entries are referred to
 * as "stale entries" in the code that follows.
 */
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value;

    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}
```

- 这个Entry是从WeakReference继承的

- ```super(k);```调用了WeakReference的构造方法

```java
Reference(T referent) {
    this(referent, null);
}
```

- **这个weakreference指向了key**

- **为什么用弱引用：防止内存泄漏**

- **当threadlocal不用了(被回收了)，map中的key == null，但这条记录并不会被回收**

- **所以在使用完threadlocal后，必须加一条**```tl.remove()```,**否则又会产生内存泄漏(VALUE还占着内存)**

- 作用

  - **Spring里有一个注解@Transactional(经常问到)**
  - 把某一个业务方法m()加上注解标记成为事务
    - m()要么全体完成，要么回滚
    - 假如m()方法里调用了两个方法m1(),m2()(m1,m2都可能去访问数据库)
    - 因为m1,m2位于同一个Transactional里面，所以必须保证他们拿到的数据库连接是同一个

  - **本质上就是用ThreadLocal来实现的(只要m1(),m2()在同一个线程里，他们拿到的就是同一个)**

- **弱引用进阶：看weakhashmap源码**

**`ThreadLocal变量类`类似于全局变量，它能降低代码的可重用性，并在类之间引入隐含的`耦合性`**

### 3.4 不变性

**不可变对象一定是线程安全的**

- 如果某个对象在被创建后其状态就不能被修改，那么这个对象就称为不可变对象。线程安全性是不可变对象的固有属性之一，它们的`不变性条件`是由`构造函数创建的`，只要它们的状态不改变，那么这些不变性调酒就能得以维持

**当满足以下条件时，对象才是不可变的**

> - 对象创建以后其状态就不能修改
> - 对象的所有域都是`final`类型（String就是如此）
> - 对象是正确创建的（在对象的创建期间，this引用没有逸出）

#### 3.4.1 Final域

- 关键字`final`可以视为C++中`const`机制的一种受限版本，用于构造`不可变性对象`
- `final`类型的域是不能修改的

**然而在`Java内存模型中`,final域还有着特殊的语义**

> `final`域能够确保`初始化过程的安全性`，从而可以不受限制地访问`不可变对象`，并在共享这些对象时无须同步

#### 3.4.2 示例：使用Volatile类型来发布不可变对象

### 3.5 安全发布

**安全发布的常用模式**：

- 可变对象必须通过安全的方式来发布，这通常意味着在`发布`和`使用`该对象的线程时都必须使用同步

> - 在静态初始化函数中初始化一个对象引用
> - 将对象的引用保存到`volatile`类型的域或者`AtomicReferance`对象中
> - 将对象的引用保存到某个正确构造对象的`final类型域中`
> - 将对象的引用保存到一个由锁保护的域中

- 对象的发布需求取决于它的可变性：

> - 不可变对象可以通过任意机制来发布
> - 事实不可变对象必须通过安全方式来发布
> - 可变对象必须通过安全方式来发布，并且必须是线程安全的或者由某个锁保护起来

- 在并发程序中使用和共享对象时，可以使用一些实用的策略

> - **线程封闭**：线程封闭的对象只能由一个线程拥有，对象被封闭在该线程中，并且只能由这个线程修改
> - **只读共享**：在没有额外同步的情况下，共享的只读对象可以由多个线程并发访问，但任何线程都不能修改它。共享的只读对象包括`不可变对象`和`事实不可变对象`
> - **线程安全共享**：线程安全的对象在其内部实现同步，因此多个线程可以通过对象的公有接口来进行访问而不需要进一步的同步。
> - **保护对象**：被保护的对象只能通过持有特定的锁来访问。保护对象包括封装在其他线程安全对象中的对象，以及已发布的并且由某个特定锁保护的对象。