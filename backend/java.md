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



## OO

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



## Interface/Abstract

|      | 接口               | 抽象类               |
| ---- | ------------------ | -------------------- |
| 方法 | 抽象               | 抽象/非抽象          |
| 变量 | final              |                      |
|      | 一个类实现多个接口 | 一个类实现一个抽象类 |
|      | 类实现接口所有方法 | 不一定               |
|      | 不能用 new 实例化  |                      |
|      | 行为规范           | 模板设计             |



## Var

|          | 成员变量                           | 局部变量                 |
| -------- | ---------------------------------- | ------------------------ |
|          | 属于类                             | 在方法中定义/参数        |
| 修饰     | 被 public,private,static 修饰      | 只有final修饰            |
| 存储     | 是对象的一部分，而对象存在于堆内存 | 存在于栈内存             |
| 生存时间 | 随着对象的创建而存在               | 随着方法的调用而自动消失 |
|          | 以类型的默认值而赋值               | 不会自动赋值             |



## == & equal

==：基本数据类型比较值，引用数据类型比较内存地址

equals() ：比较内容



## final

1. 修饰基本数据类型变量，数值在初始化后不能更改
2. 修饰引用类型变量，初始化后不能指向另一个对象
3. 修饰类，该类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法
4. 修饰方法，该方法在继承类不能被修改



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



## 4.1 OOP

**Encapsulation**

The key to making encapsulation work is to have methods never directly access
instance fields in a class other than their own. Programs should interact with object
data only through the object’s methods. Encapsulation is the way to give an object its “black box” behavior, which is the key to reuse and reliability.

```java
public String getName()
{
    return name;
}
public double getSalary()
{
    return salary;
}
```

These are obvious examples of accessor methods. As they simply return the values
of instance fields, they are sometimes called field accessors.



**Inheritance**

When you extend an existing class, the new class has all the properties and
methods of the class that you extend. You then supply new methods and data
fields that apply to your new class only. The concept of extending a class to obtain
another class is called inheritance.



**Relationships between Classes**

• Dependence (“uses–a”)

For example, the Order class uses the Account class because Order objects need
to access Account objects to check for credit status. But the Item class does not depend
on the Account class, because Item objects never need to worry about customer accounts. Thus, a class depends on another class if its methods use or manipulate
objects of that class. 

• Aggregation (“has–a”)

The aggregation, or “has–a” relationship, is easy to understand because it is concrete; for example, an Order object contains Item objects. Containment means that
objects of class A contain objects of class B.

• Inheritance (“is–a”) 

The inheritance, or “is–a” relationship, expresses a relationship between a more
special and a more general class. For example, a RushOrder class inherits from an
Order class. The specialized RushOrder class has special methods for priority handling
and a different method for computing shipping charges, but its other methods,
such as adding items and billing, are inherited from the Order class. In general, if
class A extends class B, class A inherits methods from class B but has more capabilities.



![UML notation for class relationships](..\img\UML notation for class relationships.png)



## 4.2 Predefined classes

**Construct**

To work with objects, you first construct them and specify their initial state. Then
you apply methods to the objects.
In the Java programming language, you use constructors to construct new instances.
A constructor is a special method whose purpose is to construct and initialize
objects.



## 4.3 Defning Your Own Classes

**constructor**

• A constructor has the same name as the class.
• A class can have more than one constructor.
• A constructor can take zero, one, or more parameters.
• A constructor has no return value.
• A constructor is always called with the new operator. 



**Final Instance Fields**

You can define an instance field as final. Such a field must be initialized when the
object is constructed. That is, you must guarantee that the field value has been
set after the end of every constructor. Afterwards, the field may not be modified
again.

```java
class Employee
{
    private final String name;
    . . .
}
```



## 4.4 Static

**Static Fields**

```java
class Employee
{
    private static int nextId = 1;
    private int id;
    . . .
}
```

Every employee object now has its own id feld, but there is only one nextId feld
that is shared among all instances of the class. Let’s put it another way. If there
are 1,000 objects of the Employee class, then there are 1,000 instance felds id, one for
each object. But there is a single static feld nextId. Even if there are no employee
objects, the static feld nextId is present. It belongs to the class, not to any individual
object.



**Static Constants**

```java
public class Math
{
    . . .
        public static final double PI = 3.14159265358979323846;
    . . .
}
```

If the keyword static had been omitted, then PI would have been an instance feld
of the Math class. That is, you would need an object of this class to access PI,
and every Math object would have its own copy of PI.



**Static Methods**

You can think of static methods as methods that don’t have a this parameter.

A static method of the Employee class cannot access the id instance feld because it does not operate on an object. However, a static method can access a static feld.

```java
public static int getNextId()
{
    return nextId; // returns static field
}
```

Use static methods in two situations:
• When a method doesn’t need to access the object state because all needed
parameters are supplied as explicit parameters (example: Math.pow).
• When a method only needs to access static felds of the class (example:
Employee.getNextId).



## 4.6







