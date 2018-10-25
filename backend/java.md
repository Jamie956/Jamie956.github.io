## 装箱/拆箱

|          | 装箱(Boxing)                                     | 拆箱(Unboxing)                                         |
| -------- | ------------------------------------------------ | ------------------------------------------------------ |
| 定义     | 基本数据类型 -> 包装器类型                       | 包装器类型 -> 基本数据类型                             |
| 实现     | int i = 1;<br />Integer rs = Integer.valueOf(i); | Integer i = new Integer(1);<br/>int rs = i.intValue(); |
| 自动转换 | Integer a = 1;                                   | int b = new Integer(1);                                |



Integer.valueOf()

```java
public static Integer valueOf(int i) {
    if(i >= -128 && i <= IntegerCache.high)
        return IntegerCache.cache[i + 128];
    else
        return new Integer(i);
}
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

```
父类静态代变量
父类静态代码块
子类静态变量
子类静态代码块

父类非静态变量（父类实例成员变量）
父类代码块
父类构造函数

子类非静态变量（子类实例成员变量）
子类代码块
子类构造函数
```




## 类加载

### 步骤

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



## 面向对象特性

- 封装：把一个对象的属性私有化，同时提供一些可以被外界访问的属性的方法

- 继承：使用已存在的类作为基础创建新的类，新类可以增加新的数据或新的功能，也可以用父类的功能

1. 子类拥有父类非 private 的属性和方法
2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展
3. 子类可以用自己的方式实现父类的方法

- 多态：在Java中有两种形式实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）

- 重载：发生在同一个类中，方法名相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同

- 重写：发生在父子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为 private 则子类就不能重写该方法



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



## 面向对象

- 可复用，共同点的抽象
- 可扩展，继承
- 灵活性好
- 把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数
- 面向对象的设计思想是抽象出Class，根据Class创建Instance



- 面向过程：把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。



- 面向对象：把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。



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

   

## ArrayList





























