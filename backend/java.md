## 装箱/拆箱

|          | 装箱(Boxing)                                     | 拆箱(Unboxing)                                         |
| -------- | ------------------------------------------------ | ------------------------------------------------------ |
| 定义     | 将基本数据类型转换为包装器类型                   | 将包装器类型转换为基本数据类型                         |
| 实现     | int i = 1;<br />Integer rs = Integer.valueOf(i); | Integer i = new Integer(1);<br/>int rs = i.intValue(); |
| 自动转换 | Integer a = 1;                                   | int b = new Integer(1);                                |



- Integer.valueOf()

```java
public static Integer valueOf(int i) {
    if(i >= -128 && i <= IntegerCache.high)
        return IntegerCache.cache[i + 128];
    else
        return new Integer(i);
}
```



### Boxing, int -> Integer

```java
int i = 1;
Integer rs = Integer.valueOf(i);
```

### Unboxing, Integer -> int

```java
Integer i = new Integer(1);
int rs = i.intValue();
```





## 对象创建过程

1. new 类名
2. 根据new的参数在常量池中定位一个类的符号引用
3. 如果没有找到这个符号引用，说明类还没被加载，则进行类的加载、解析和初始化
4. 虚拟机为对象分配堆内存
5. 将分配的内存初始化为零值
6. 设置对象头，Mark Word (HashCode、GC 分代年龄、锁状态标志、线程持有的锁、偏向线程 ID、偏向时间戳、对象分代年龄)，类型指针
7. 调用对象的init方法



### 执行顺序

#### v1
1. 执行全部静态代码块

2. 如果继承父类，执行父类的代码块

3. 如果继承父类，执行父类默认无参构造方法

4. 执行代码块

5. 执行默认无参构造方法

#### v2
```
父类静态代变量
父类静态代码块
子类静态变量
子类静态代码块

父类非静态变量（父类实例成员变量）
父类构造函数
子类非静态变量（子类实例成员变量）
子类构造函数
```

#### v3
1. 执行全部静态代码块

2. 执行父类的代码块

3. 执行父类构造方法

4. 执行子类代码块

5. 执行子类构造方法


## 类加载

1. 装载：由类加载器完成，将.class文件的二进制文件载入JVM的方法区，并且在堆区创建描述这个类的java.lang.Class对象，作为该类访问入口
2. 链接：分三步			
  1. 校验：确认此二进制文件是否适合当前的JVM
  2. 准备：为静态成员分配内存空间，并设置默认值
  3. 解析：转换常量池中的代码作为直接引用的过程，直到所有的符号引用都可以被运行程序使用（建立完整的对应关系）
3. 初始化：执行类中定义的java程序代码（类构造器）
4. 卸载：没有任何引用指向Class对象时就会被卸载



### 加载器

- 启动类加载器 Bootstrap ClassLoader：加载<JAVA_HOME>\lib目录下核心库

- 扩展类加载器 Extension ClassLoader：加载<JAVA_HOME>\lib\ext目录下扩展包

- 应用程序类加载器 Application ClassLoader：  加载用户路径(classpath)上指定的类库



### 双亲委派模型

如果一个类加载器收到类加载的请求，它不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成

1. 当Application ClassLoader 收到一个类加载请求时，他首先不会自己去尝试加载这个类，而是将这个请求委派给父类加载器Extension ClassLoader去完成

2. 当Extension ClassLoader收到一个类加载请求时，他首先也不会自己去尝试加载这个类，而是将请求委派给父类加载器Bootstrap ClassLoader去完成

3. 如果Bootstrap ClassLoader加载失败(在<JAVA_HOME>\lib中未找到所需类)，就会让Extension ClassLoader尝试加载

4. 如果Extension ClassLoader也加载失败，就会使用Application ClassLoader加载

5. 如果Application ClassLoader也加载失败，就会使用自定义加载器去尝试加载

6. 如果均加载失败，就会抛出ClassNotFoundException异常



## 封装

把一个对象的属性私有化，同时提供一些可以被外界访问的属性的方法



## 继承

使用已存在的类作为基础建立新的类，新类可以增加新的数据或新的功能，也可以用父类的功能

1. 子类拥有父类非 private 的属性和方法
2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展
3. 子类可以用自己的方式实现父类的方法



## 多态

指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定
在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）



## 重载

发生在同一个类中，方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同



## 重写

发生在父子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为 private 则子类就不能重写该方法



## String

|          | String                                                       | StringBuilder                                                | StringBuffer                             |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------------------- |
| 可变     | N                                                            | Y                                                            | Y                                        |
| 线程安全 | Y                                                            | N                                                            | Y(同步锁)                                |
|          | 创建 String 类型的对象时，虚拟机到常量池查找是否有相同的值的对象，如果有就赋值给当前引用，否则在常量池创建一个String对象 | 比使用 StringBuffer 获得 10%~15% 左右的性能提升，但有线程安全问题 | 每次都会对 StringBuffer 对象本身进行操作 |
| 应用场景 | 操作少量的数据                                               | 单线程操作字符串缓冲区下操作大量数据                         | 多线程操作字符串缓冲区下操作大量数据     |



### API

```
string.toUpperCase(); 大写
string.toLowerCase(); 大写
str1.contains(str2) 是否包含
str.indexOf(key) 根据key获取索引
str.split(";"); String -> Array
String.join(" + ", array); Array -> String
string1.concat(string2) 连接字符串
```



## 接口/抽象类

|      | 接口               | 抽象类               |
| ---- | ------------------ | -------------------- |
| 方法 | 抽象               | 抽象/非抽象          |
| 变量 | final              |                      |
|      | 一个类实现多个接口 | 一个类实现一个抽象类 |
|      | 类实现接口所有方法 | 不一定               |
|      | 不能用 new 实例化  |                      |
|      | 行为规范           | 模板设计             |



## 变量

|          | 成员变量                           | 局部变量                 |
| -------- | ---------------------------------- | ------------------------ |
|          | 属于类                             | 在方法中定义/参数        |
| 修饰     | 被 public,private,static 修饰      | 只有final修饰            |
| 存储     | 是对象的一部分，而对象存在于堆内存 | 存在于栈内存             |
| 生存时间 | 随着对象的创建而存在               | 随着方法的调用而自动消失 |
|          | 以类型的默认值而赋值               | 不会自动赋值             |



## 构造方法

1. 名字与类名相同
2. 没有返回值，不能用void声明
3. 生成类的对象时自动执行，无需调用



## == & equal

==：基本数据类型比较值，引用数据类型比较内存地址

equals() ：比较内容



## **线程**

### 定义

在进程中独立运行的子任务

一个进程在其执行的过程中可以产生多个线程，同一个进程中的线程共享其进程中的内存和资源（共享的内存是堆内存和方法区内存，栈内存不共享），相比进程，线程之间的切换效率较高



### 生命周期

1. **新建(new)**：创建Thread实例时，此线程进入新建状态（未被启动）

   ```Thread t1 = new Thread();```

2. **就绪(runnable)**：线程已经被启动，正在等待被分配给CPU时间片，此时线程正在就绪队列中排队等候得到CPU资源

   ```t1.start();```

3. **运行(running)**：线程获得CPU资源正在执行任务，一直运行到结束，除非自动放弃CPU资源或者有优先级更高的线程进入

4. **阻塞(block)**：由于某种原因导致正在运行的线程让出CPU并暂停自己的执行

      1.  sleep：在指定时间后可进入就绪状态
      2.  wait：线程放入等待队列(waitting queue)中

5. **死亡(dead)**：线程执行完毕或被其它线程杀死时，进入死亡状态，这时不能再进入就绪状态

   1.  自然终止：正常运行run()方法后终止
   2.  异常终止：调用**stop()**方法让一个线程终止运行



<img src="https://camo.githubusercontent.com/5b764ff5af6204f82c7ae6237b20c41f9505aef8/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f392f313635316631396437633465393361333f773d38373626683d34393226663d706e6726733d313238303932" alt="https://camo.githubusercontent.com/5b764ff5af6204f82c7ae6237b20c41f9505aef8/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f392f313635316631396437633465393361333f773d38373626683d34393226663d706e6726733d313238303932" class="transparent">



### wait/notify

- wait 让当前线程进入等待状态，并且释放锁
- notify 唤醒任意一个正在等待锁的线程，并且让它得到锁
- notifyAll 唤醒所有等待对象锁的线程，如果有多个线程都被唤醒，那么锁将会被他们争夺，同一时间只会有一个线程得到锁



### 常用方法

- `hread.currentThread()` 静态方法，获取当前线程对象；
- `isAlive()` 判断线程是否处于活动状态，即线程已启动且尚未终止；
- `Thread.sleep(long)` 在指定的毫秒数内让当前线程休眠，需要`catch InterruptedException`；
- `Thread.interrupted()` 判断该线程是否中断，执行后将清除中断标志；
- `isInterrupted()` 测试线程是否已经中断；
- `suspend()   resume()  stop()` 暂停、开始和结束线程，不应该使用。暂停方法不会释放资源；
- `yield()` 该线程放弃当前CPU资源，放弃后马上进行CPU竞争；
- `setPriority()`优先级具有继承性，即子线程有父线程的优先级；
  - 高优先级的线程总是大部分先执行完，但不代表高优先级将全部先完成；
  - 优先级较高的不一定每一次都先执行完，具有随机性；
  - 具体的与OS相关；
- `setDaemon()` 设置守护线程，当进程不存在非守护线程时则守护线程将销魂而后进程销毁；



### 非线程安全

存在多个线程对同一个对象中的实例变量进行并发访问控制由此导致的数据脏读；



## 线程池

### 优点

1. 减少创建和销毁线程的次数，重复使用线程
2. 根据系统的承受能力，调整线程池中工作线程的数量，防止消耗过多内存导致服务器崩溃



### ThreadPoolExecutor

- corePoolSize：默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中
- maximumPoolSize：线程池最大线程数，表示在线程池中最多能创建多少个线程
- keepAliveTime：表示线程没有任务执行时最多保持多久时间会终止。默认情况下，只有当线程池中的线程数大于corePoolSize时，keepAliveTime才会起作用，直到线程池中的线程数不大于corePoolSize
- unit：参数keepAliveTime的时间单位
- workQueue：一个阻塞队列，用来存储等待执行的任务
- threadFactory：线程工厂，主要用来创建线程
- handler：超出线程范围和队列容量的任务的处理程序，任务拒绝策略
- poolSize：当前的线程数
- mainLock：状态锁，对线程池状态（比如线程池大小，runState等）的改变都要使用这个锁
- workers：用来存放工作集
- allowCoreThreadTimeOut：是否允许为核心线程设置存活时间
- largestPoolSize： 曾经出现过的最大线程数
- completedTaskCount：执行完毕的任务个数

### 实现原理

- 线程池状态
- 任务的执行
  - 线程池线程数 < corePoolSize，来一个任务，就创建一个线程去执行这个任务
  - 线程池线程数 >= corePoolSize，来一个任务，尝试将它添加到任务缓存队列，如果添加成功，就等待空闲线程将它取出，否则（一般是任务缓存队列满了），就会尝试创建新线程执行这个任务
  - 线程池线程数 = maximumPoolSize，采取任务拒绝策略
  - 线程池线程数 > corePoolSize，如果线程空闲时间超过keepAliveTime，就会被终止，直到 线程池线程数 < corePoolSize

- 线程池中的线程初始化

- 任务缓存队列及排队策略

- 任务拒绝策略

  当线程池的任务缓存队列已满并且线程池中的线程数目达到maximumPoolSize，如果还有任务到来就会采取任务拒绝策略

```
ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。
ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。
ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务 
```

- 线程池的关闭
  - shutdown()：不会立即终止线程池，等缓存队列中的任务执行完才终止，且不接受新任务
  - shutdownNow()：立即终止线程池，尝试打断正在执行的任务，清空任务缓存队列，返回尚未执行的任务
- 线程池容量的动态调整

### 配置线程池大小

- 如果是CPU密集型任务，就需要尽量压榨CPU，参考值可以设为 *N*CPU+1
- 如果是IO密集型任务，参考值可以设置为2**N*CPU



### 创建

```java
ThreadPoolExecutor.ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue)

threadPoolExecutor.execute(thread);
threadPoolExecutor.shutdown();

```





## 锁

### 乐观锁

> 每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据。乐观锁适用于多读的应用类型，提高吞吐量

#### CAS

> CAS是乐观锁技术，当多个线程尝试使用CAS同时更新同一个变量时，只有其中一个线程能更新变量的值，而其它线程都失败，失败的线程并不会被挂起，而是被告知这次竞争中失败，并可以再次尝试。



> CAS 操作中包含三个操作数 —— 需要读写的内存位置（V）、进行比较的预期原值（A）和拟写入的新值(B)。如果内存位置V的值与预期原值A相匹配，那么处理器会自动将该位置值更新为新值B。否则处理器不做任何操作。

### 悲观锁

> 总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。Java的synchronized关键字的实现也是悲观锁。

#### 存在问题

1. 在多线程竞争下，加锁、释放锁会导致比较多的上下文切换和调度延时，引起性能问题
2. 一个线程持有锁会导致其它所有需要此锁的线程挂起
3. 如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置，引起性能风险



## **程序**

含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码



## **进程**

每个进程是一个应用程序，都有独立的内存空间



是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如CPU时间，内存空间，文件，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。



## final

修饰：变量、方法、类。

1. 修饰变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象
2. 修饰类，这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法
3. 修饰方法，防任何继承类修改它



## Servlet

### 生命周期

1. 初始化：调用init()，只执行一次
2. 请求：每次收到请求，调用service()，它会根据需要调用与请求对应的doGet()或doPost()
3. 销毁：服务器关闭或项目被卸载时，调用destroy()，只执行一次



## static

1. **修饰成员变量/方法:** 被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享，静态变量 存放在 Java 内存区域的方法区。调用格式：`类名.静态变量名`    `类名.静态方法名()` 
2. **静态代码块:** 执行顺序 静态代码块 -> 非静态代码块 -> 构造方法，该类不管创建多少对象，静态代码块只执行一次
3. **静态内部类（static修饰类的话只能修饰内部类）：** 静态内部类与非静态内部类之间存在一个最大的区别:  非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围内，但是静态内部类却没有。没有这个引用就意味着：1.  它的创建是不需要依赖外围类的创建。2. 它不能使用任何外围类的非static成员变量和方法。
4. **静态导包(用来导入类中的静态资源，1.5之后的新特性):** 格式为：`import static` 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。



|          | 静态方法                                            | 非静态（实例）方法     |
| -------- | --------------------------------------------------- | ---------------------- |
|          | 属于类本身                                          | 属于从类生成的每个对象 |
| 应用场景 | 方法执行的操作不依赖于其类的各个变量和方法          | 否则                   |
| 访问限制 | 访问本类成员时，只允许访问静态成员（静态变量/方法） | 无                     |
| 调用格式 | 类名.方法名/对象名.方法名                           | 对象名.方法名          |



## this

用于引用类的当前实例



## super

子类访问父类的变量和方法



## Arraylist & LinkedList

|              | Arraylist                         | LinkedList           |
| ------------ | --------------------------------- | -------------------- |
| 线程安全     | N                                 | N                    |
| 底层数据结构 | 数组                              | 双向循环链表         |
| 复杂度       | 末尾添加 O(1)，指定位置增删O(n-i) | O(1)                 |
| 随机元素访问 | Y                                 | N                    |
| 内存占用     | 末尾预留空间                      | 每个元素存放前后节点 |



## 题目

1. 

```java
Integer a = 1;
Integer b = 2;
Integer c = 3;
Integer d = 3;
Integer e = 321;
Integer f = 321;
Long g = 3L;
Long h = 2L;

System.out.println(c==d);// true 比较对象，指向cache
System.out.println(e==f);// false 比较对象
System.out.println(c==(a+b));// true 存在表达式，拆箱比较数值
System.out.println(c.equals(a+b));// true 比较数值
System.out.println(g==(a+b));// true 存在表达式，拆箱比较数值
System.out.println(g.equals(a+b));// false a+b调用Integer.valueOf拆箱
System.out.println(g.equals(a+h));// true a+h调用Long.valueOf向上转型
```

解析：

- "=="运算符
  - 两个操作数都是包装器类型的引用 =》比较地址
  - 存在表达式 =》比较数值（自动拆箱）

- equals()不会类型转换



2. 

```java
Double i1 = 100.0;
Double i2 = 100.0;
Double i3 = 200.0;
Double i4 = 200.0;

System.out.println(i1==i2);// false 浮点的值不是有限的
System.out.println(i3==i4);// false
```



## Design Pattern

### Singleton

定义，保证一个类仅有一个实例，并提供一个访问它的全局访问点。
要点

- 单例类必须要有一个 private 访问级别的构造函数，只有这样，才能确保单例不会在系统中的其他代码内被实例化;
- instance 成员变量和 uniqueInstance 方法必须是 static 的。

饿汉方式。指全局的单例实例在类装载时构建
懒汉方式。指全局的单例实例在第一次被使用时构建。



## 面向对象编程

- 可复用
- 可扩展
- 灵活性好
- 把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数
- 面向对象的设计思想是抽象出Class，根据Class创建Instance

### 个人理解

- 把万物的共性抽象出来，比如打工仔，老板，乞丐都是人，人就是他们的共同点，是因为他们具有人的基本特征：眼睛，手指，膝盖等等，把这些特征称作属性，当我们创建人的时候，就会考虑眼睛有多大，手指有多长，这就相当于属性的值，这个我们创建的人就是一个有他自身特点的个体，同理，可以创建第二个，第三个，他们都是基于人这个对象，通过给属性赋值，创建出来，这就说明了对象的复用性

```java
Class Person{
    int eyes;
    int finger;
    Person(int eyes,int finger){
        this.eyes = eyes;
        this.finger = finger;
    }
}

Person p = new Person(3,100);
Person p2 = new Person(5,362);
```

- 打工仔和老板虽然都是人，但是他们有他们自身的特点，不完全是一类人，这时，我们可以对对象进行扩展，既然打工仔是人，老板也是人，那么就可以打工仔继承人，老板继承人，拥有人的共同属性，但是他们又拥有自身的属性，同理，打工仔可以继续细分下去，这说明了对象的扩展性

```java
Class Person{}
Class Boss extends Person{}
Class Employee extends Person{}
```

- 除了继承，还可以把对象的共同行为归纳在一起，称作接口，如果一个类实现了接口，就相当于拥有接口的一套行为规范，比如打工仔和老板都是在同一间公司，写同一个项目的代码。

### 面向过程 & 面向对象

- 面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。
- 面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。



## Array

- 定义一个指定长度的数组

  ```java
  int[] a = new int[3];
  ```

- 定义一个包含元素的数组

  ```java
  int[] b = { 1, 2, 3 };
  int[] c = new int[] { 1, 2, 3 };
  ```

- 定义一个指定长度的二维数组

  ```java
  int[][] f = new int[5][4];
  ```

- 定义一个包含元素的二维数组 

  ```java
  int[][] d = { { 1, 2 }, { 3, 4, 5 } };
  ```

- 遍历

  ```for (T element : array) {}```

- Array -> List

  ```Arrays.asList(array)```



## Socket

- Socket：网络上运行的程序之间双向通信链路的终结点，是TCP和UDP的基础，由IP和Port组成
- Socket套接字
- Socket原理机制
  - 通信的两端都有Socket
  - 数据在两个Socket间通过IO传输

### Server

1. new ServerSocket(int port)，创建Server并指定监听端口
2. accept() 接受Socket
3. 从Socket类获取输入流
4. 读取输入流
5. 指定编码
6. 存入StringBuilder
7. 从Socket类获取输出流
8. 写出信息
9. 关闭Server

### Client

1. new Socket(String host, int port) 连接到Socket Server
2. 从Socket获取输出流
3. 往输出流写入信息
4. shutdownOutput() 告诉服务器发送完数据
5. 从Socket获取输入流
6. 读取输入流
7. 指定编码
8. 存入StringBuilder
9. 关闭Server



## 反射

### 作用

1. 在运行时判断任意一个对象所属的类；
2. 在运行时获取类的对象；
3. 在运行时访问java对象的属性，方法，构造方法等。

### API

- 具体实例 -> 包名类名

```java
myInstance.getClass().getName()
```

- 包名类名 -> Class实例

```java
Class.forName("com.example.MyInstance");
```

- Class实例 -> 创建具体实例

```java
(MyInstance) classInstance.newInstance();
```

- Class实例 -> 获取构造函数

```java
classInstance.getConstructors();
```

- 构造函数 -> 创建具体实例

```java
(MyInstance) constructors[0].newInstance();
```

- Class实例 -> 获取实现的接口

```java
classInstance.getInterfaces();
```

- Class实例 -> 继承的父类

```java
classInstance.getSuperclass();
```

- Class实例 -> 成员变量

```java
classInstance.getDeclaredFields();
classInstance.getDeclaredFields("name");
```

- Class实例 -> 父类或接口的变量

```java
classInstance.getFields();
```

- fields[i] -> 权限修饰符

```java
Modifier.toString(fields[i].getModifiers());

```

- fields[i] -> 类型

```java
fields[i].getType().getName()
```

- Class实例 -> Method实例

```java
classInstance.getMethod("hello");
classInstance.getMethod("hello", String.class);
```

- Method实例 -> Method调用

```java
method.invoke(classInstance.newInstance());
method.invoke(classInstance.newInstance(), "tom");
```

- 操作属性

```java
field.setAccessible(true);
field.set(myInstance,"cat");
```

- 获取类加载器类型

```java
myInstance.getClass().getClassLoader().getClass().getName()
```

- 动态代理结构

1. 定义实体（要实现接口）
2. 自定义InvocationHandler，需要实现接口InvocationHandler
3. 重写方法invoke
4. 获取代理对象，Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)



### 注意

- 即使同一个class创建多个实例，他们的静态成员变量是共享的



## BigDecimal

### 比较大小

```java
bigDecimal.compareTo(bigDecimal);
```

- 1 >
- 0 =
- -1 <

### 运算

```java
bigDecimal.add(bigDecimal);//加
bigDecimal.subtract(bigDecimal);//减
bigDecimal.multiply(bigDecimal);//乘
bigDecimal.multiply(bigDecimal, new MathContext(4, RoundingMode.HALF_DOWN));//保留位数
bigDecimal.divide(bigDecimal,10,RoundingMode.HALF_UP);//除，必须保留位数
bigDecimal.remainder(bigDecimal);//余数
bigDecimal.pow(n);//n次方
bigDecimal.max(bigDecimal);//最大值
bigDecimal.min(bigDecimal);//最小值
bigDecimal.movePointLeft(n);//小数点左移n位
bigDecimal.movePointRight(n);//小数点右移n位
```



## BigInteger

```java
//long/int -> BigInteger
BigInteger.valueOf(n);


//string -> BigInteger
new BigInteger(string);

//10进制 -> 2进制
new BigInteger(binaryString , 2);

```



## File

```new File("D:\\a.txt")``` 绝对路径创建实例
```isDirectory()``` 是否是文件夹
```mkdir()``` 创建文件夹
```createNewFile()``` 创建文件
```isFile()``` 是否是文夹
```mkdirs()``` 创建层级文件夹
```renameTo(File)``` 重命名
```delete()``` 删除文件
```getName()``` 获取文件名或文件夹名
```getAbsolutePath()``` 获取绝对路径
```list()``` 获取绝对路径
```listFiles()``` 获取抽象路径数组



## IO

```new FileInputStream(File);``` 实例化文件输入流
```fileInputStream.read();fileInputStream.read(new byte[20]);``` 读取输入流
```new FileOutputStream(File);``` 实例化文件输出流
```fileOutputStream.write(byte[]);``` 写出输出流
```fileOutputStream.write(byte[], 0, len);```
```fileOutputStream.flush();```



## Java 8 - Time

```LocalDate.now();``` 获取当天格式化日期
```LocalDate.of(2018, 05, 20);``` 获取指定格式化日期
```Clock.systemUTC();```
```Clock.systemDefaultZone();``` 获取系统时区
```MonthDay.of(5, 20);```
```LocalTime.now();``` 获取当前时间
```YearMonth.now();``` 获取当月天数
```Instant.now();``` 获取时间戳
```LocalDate.parse(text, formatter)``` 解析日期
```LocalDate.format(formatter)``` 格式化日期



## NumberFormat

```java
//格式化数字
NumberFormat.getInstance(new Locale("en", "IN")).format(10000000.99);
//货币格式化
NumberFormat.getCurrencyInstance(new Locale("en", "IN")).format(10340.999);
//百分比格式化
NumberFormat.getPercentInstance(new Locale("en", "IN")).format(10340.999);
```



## Regexp

| 字符   | 描述                                                         |
| :----- | ------------------------------------------------------------ |
| $      | 匹配输入字符串的结尾位置                                     |
| ()     | 标记一个子表达式的开始和结束位置                             |
| {}     | 标记限定符表达式的开始                                       |
| \w     | 匹配包括下划线的任何单词字符,等价于“[A-Za-z0-9_]”            |
| \d     | 匹配一个数字字符。等价于[0-9]                                |
| \s     | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v] |
| \D     | 匹配一个非数字字符。等价于[^0-9]                             |
| \W     | 匹配任何非单词字符。等价于[A-Za-z0-9]                        |
| []     | 字符范围，匹配指定范围内的任意字符                           |
| [xyz]  | 字符集合。匹配所包含的任意一个字符                           |
| [^xyz] | 负值字符集合。匹配未包含的任意字符。                         |
| [a-z]  | 字符范围。匹配指定范围内的任意字符                           |
| .      | 匹配除“\n”之外的任何单个字符                                 |
| *      | 匹配前面的子表达式零次或多次                                 |
| +      | 匹配前面的子表达式一次或多次                                 |
| ?      | 贪婪模式则尽可能多的匹配所搜索的字符串                       |
| {n}    | n是一个非负整数。匹配确定的n次                               |
| {n,}   | 至少匹配n次                                                  |
| {n,m}  | m和n均为非负整数，其中n<=m。最少匹配n次且最多匹配m次         |



### 例子

|              |                                    |
| ------------ | ---------------------------------- |
| /\d/         | 匹配数字，无限定符                 |
| /\d+/        | 匹配数字，一次或多次               |
| /\d+/g       | 匹配数字，全局                     |
| /[abc]       | 匹配包含a,b,c                      |
| /[a-z]/g     | 匹配包含a-z，全局                  |
| /[^a-z]/g    | 不匹配a-z的字符                    |
| /b/          | 匹配特定一个字符                   |
| /[^a-z0-9]/g | 不匹配a-z和0-9的字符               |
| /a\|2\|o/g   | 匹配多个指定字符                   |
| /<.*>/       | 匹配从 < 到 > 之间的所有内容       |
| /<.*?>/      | 匹配从 < 到第一个 > 之间的所有内容 |
| /<\w+?>/     | 匹配开始的 < >                     |



## 生产者/消费者

### 生产者

```java
//继承Thread，重写run()
run(){
    while(true){
        synchronized (queue) {
            //超过限制
            while(maxSize){
                //释放锁
                wait()
            }
            //生产
            notifyAll()
        }
    }
}
```

### 消费者

```java
//继承Thread，重写run()
run(){
	while(true){
		synchronized (queue) {
			//消费队列为空
			while(empty){
				//释放锁
				wait()
			}
			//消费
			notifyAll()
		}
	}	
}
```



## 阅读源码

### eclipse查看源码

1. “window”-> “Preferences” -> “Java” -> “Installed JREs”
2. ...rt.jar -> Source attachment:(none) -> extends file -> ./jdk/src.zip



### 如何分析

1. 看顶部注释

2. 总结

3. type hierarchy显示类的继承和实现结构

4. 查看继承类和实现接口

   































