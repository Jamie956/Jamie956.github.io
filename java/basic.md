# Basic


# java



不提供指针访问内存

JRE（Java Runtime Environment）

字节码（.class 文件）

Java 源代码---->编译器---->jvm 可执行的 Java 字节码(即虚拟指令)---->jvm---->jvm 中解释器----->机器可执行的二进制机器码---->程序运行

装箱：将基本类型用它们对应的引用类型包装起来；
int -> integer

拆箱：将包装类型转换为基本数据类型；
integer -> int

静态方法可以不通过对象进行调用



new运算符创建实例，对象放在堆内存，对象引用放在栈内存，

对象相等，指内存中存放的内容

引用相等，指内存地址



## 特性

```
封装
把一个对象的属性私有化，同时提供一些可以被外界访问的属性的方法

继承
使用已存在的类作为基础建立新的类，新类可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类
  1. 子类拥有父类非 private 的属性和方法
  2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展
  3. 子类可以用自己的方式实现父类的方法

多态
指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定
在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）
```



## 重载，重写

```
重载： 发生在同一个类中，方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同，发生在编译时　　

重写： 发生在父子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为 private 则子类就不能重写该方法
```



## String

|          | String                                                       | StringBuilder                                                | StringBuffer                                          |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------- |
| 可变     | N                                                            | Y                                                            | Y                                                     |
| 线程安全 | Y                                                            | N                                                            | Y(同步锁)                                             |
|          | 每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象 | StirngBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险 | StringBuffer 每次都会对 StringBuffer 对象本身进行操作 |
| 使用选择 | 操作少量的数据                                               | 单线程操作字符串缓冲区下操作大量数据                         | 多线程操作字符串缓冲区下操作大量数据                  |



当创建 String 类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 String 对象。



## 接口，抽象类

|      | 接口               | 抽象类           |
| ---- | ------------------ | ---------------- |
| 方法 | 抽象               | 抽象/非抽象      |
| 变量 | final              |                  |
|      | 类实现多个接口     | 类实现一个抽象类 |
|      | 类实现接口所有方法 | 不一定           |
|      | 不能用 new 实例化  |                  |
|      | 行为规范           | 模板设计         |



## 变量

|          | 成员变量                           | 局部变量                 |
| -------- | ---------------------------------- | ------------------------ |
|          | 属于类                             | 在方法中定义/参数        |
| 修饰     | 被 public,private,static 修饰      | 只有final修饰            |
| 存储     | 是对象的一部分，而对象存在于堆内存 | 存在于栈内存             |
| 生存时间 | 随着对象的创建而存在               | 随着方法的调用而自动消失 |
|          | 以类型的默认值而赋值               | 不会自动赋值             |



## 构造方法

1. 名字与类名相同；

2. 没有返回值，但不能用void声明构造函数；

3. 生成类的对象时自动执行，无需调用。

   

## 静态/实例方法

|      | 静态方法                  | 实例方法      |
| ---- | ------------------------- | ------------- |
| 调用 | 类名.方法名/对象名.方法名 | 对象名.方法名 |
|      | 只允许访问静态成员        | 无限制        |



## ==，equal

**==** : 比较地址(基本数据类型比较值，引用数据类型内存地址)

**equals()** :比较内容



## **线程**

比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。



1.  **新建(new)**：新创建了一个线程对象。

2. **可运行(runnable)**：线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获 取cpu的使用权。

3. **运行(running)**：可运行状态(runnable)的线程获得了cpu时间片（timeslice），执行程序代码。

4. **阻塞(block)**：阻塞状态是指线程因为某种原因放弃了cpu使用权，也即让出了cpu  timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有 机会再次获得cpu  timeslice转到运行(running)状态。阻塞的情况分三种： 

   (一). 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放 入等待队列(waitting queue)中。 

   (二). 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁 被别的线程占用，则JVM会把该线程放入锁池(lock  pool)中。 

   (三). 其他阻塞: 运行(running)的线程执行Thread.sleep(long  ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态。

5. **死亡(dead)**：线程run()、main()方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期。死亡的线程不可再次复生。



<img src="https://camo.githubusercontent.com/5b764ff5af6204f82c7ae6237b20c41f9505aef8/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f392f313635316631396437633465393361333f773d38373626683d34393226663d706e6726733d313238303932" alt="https://camo.githubusercontent.com/5b764ff5af6204f82c7ae6237b20c41f9505aef8/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f392f313635316631396437633465393361333f773d38373626683d34393226663d706e6726733d313238303932" class="transparent">



## **程序**

是含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码。



## **进程**

是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如CPU时间，内存空间，文件，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。



## final

修饰：变量、方法、类。

1. 修饰变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。
2. 修饰类，这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法。
3. 修饰方法，防任何继承类修改它



## Servlet

**生命周期：** **Web容器加载Servlet并将其实例化后，Servlet生命周期开始**，容器运行其**init()方法**进行Servlet的初始化；请求到达时调用Servlet的**service()方法**，service()方法会根据需要调用与请求对应的**doGet或doPost**等方法；当服务器关闭或项目被卸载时服务器会将Servlet实例销毁，此时会调用Servlet的**destroy()方法**。**init方法和destory方法只会执行一次，service方法客户端每次请求Servlet都会执行**。Servlet中有时会用到一些需要初始化与销毁的资源，因此可以把初始化资源的代码放入init方法中，销毁资源的代码放入destroy方法中，这样就不需要每次处理客户端的请求都要初始化与销毁资源。



## **static**

1. **修饰成员变量/方法:** 被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享，静态变量 存放在 Java 内存区域的方法区。调用格式：`类名.静态变量名`    `类名.静态方法名()` 
2. **静态代码块:** 静态代码块定义在类中方法外, 静态代码块在非静态代码块之前执行(静态代码块—>非静态代码块—>构造方法)。该类不管创建多少对象，静态代码块只执行一次.
3. **静态内部类（static修饰类的话只能修饰内部类）：** 静态内部类与非静态内部类之间存在一个最大的区别:  非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围内，但是静态内部类却没有。没有这个引用就意味着：1.  它的创建是不需要依赖外围类的创建。2. 它不能使用任何外围类的非static成员变量和方法。
4. **静态导包(用来导入类中的静态资源，1.5之后的新特性):** 格式为：`import static` 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。

SUM

- 静态方法属于类本身，非静态方法属于从该类生成的每个对象。 如果您的方法执行的操作不依赖于其类的各个变量和方法，请将其设置为静态（这将使程序的占用空间更小）。 否则，它应该是非静态的。
- 静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），而不允许访问实例成员变量和实例方法；实例方法则无此限制



## this

用于引用类的当前实例



## super

用于子类访问父类的变量和方法





## Arraylist & LinkedList

|              | Arraylist                         | LinkedList           |
| ------------ | --------------------------------- | -------------------- |
| 线程安全     | N                                 | N                    |
| 底层数据结构 | 数组                              | 双向循环链表         |
| 复杂度       | 末尾添加 O(1)，指定位置增删O(n-i) | O(1)                 |
| 随机元素访问 | Y                                 | N                    |
| 内存占用     | 末尾预留空间                      | 每个元素存放前后节点 |


