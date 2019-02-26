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



## List

|              | Arraylist                         | LinkedList           |
| ------------ | --------------------------------- | -------------------- |
| 线程安全     | N                                 | N                    |
| 底层数据结构 | 数组                              | 双向循环链表         |
| 复杂度       | 末尾添加 O(1)，指定位置增删O(n-i) | O(1)                 |
| 随机元素访问 | Y                                 | N                    |
| 内存占用     | 末尾预留空间                      | 每个元素存放前后节点 |



## proxy

1. 在运行时判断任意一个对象所属的类；
2. 在运行时获取类的对象；
3. 在运行时访问java对象的属性，方法，构造方法等。



## 4 Objects and Classes



### 4.1 Introduction to Object-Oriented Programming

**Encapsulation**

The key to making encapsulation work is to have methods never directly access instance fields in a class other than their own. Programs should interact with object data only through the object’s methods. 

Encapsulation is the way to give an object its “black box” behavior, which is the key to reuse and reliability.

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

These are obvious examples of accessor methods. As they simply return the values of instance fields, they are sometimes called field accessors.



**Inheritance**

When you extend an existing class, the new class has all the properties and methods of the class that you extend. You then supply new methods and data fields that apply to your new class only. The concept of extending a class to obtain another class is called inheritance.



**Relationships between Classes**

• Dependence (“uses–a”)

For example, the Order class uses the Account class because Order objects need to access Account objects to check for credit status. But the Item class does not depend on the Account class, because Item objects never need to worry about customer accounts. Thus, a class depends on another class if its methods use or manipulate objects of that class. 

• Aggregation (“has–a”)

The aggregation, or “has–a” relationship, is easy to understand because it is concrete; for example, an Order object contains Item objects. Containment means that objects of class A contain objects of class B.

• Inheritance (“is–a”) 

The inheritance, or “is–a” relationship, expresses a relationship between a more special and a more general class. For example, a RushOrder class inherits from an Order class. The specialized RushOrder class has special methods for priority handling and a different method for computing shipping charges, but its other methods, such as adding items and billing, are inherited from the Order class. In general, if class A extends class B, class A inherits methods from class B but has more capabilities.



![UML notation for class relationships](..\img\UML notation for class relationships.png)



### 4.2 Using Predefined classes

**Construct**

To work with objects, you first construct them and specify their initial state. Then you apply methods to the objects.

In the Java programming language, you use constructors to construct new instances.

A constructor is a special method whose purpose is to construct and initialize objects.



### 4.3 Defning Your Own Classes

**constructor**

• A constructor has the same name as the class.
• A class can have more than one constructor.
• A constructor can take zero, one, or more parameters.
• A constructor has no return value.
• A constructor is always called with the new operator. 



**Final Instance Fields**

You can define an instance field as final. Such a field must be initialized when the object is constructed. That is, you must guarantee that the field value has been set after the end of every constructor. Afterwards, the field may not be modified again.

```java
class Employee
{
    private final String name;
    . . .
}
```



### 4.4 Static Field and Methods

**Static Fields**

```java
class Employee
{
    private static int nextId = 1;
    private int id;
    . . .
}
```

Every employee object now has its own id feld, but there is only one nextId feld that is shared among all instances of the class. Let’s put it another way. If there are 1,000 objects of the Employee class, then there are 1,000 instance felds id, one for each object. But there is a single static feld nextId. Even if there are no employee objects, the static feld nextId is present. It belongs to the class, not to any individual object.



**Static Constants**

```java
public class Math
{
    . . .
        public static final double PI = 3.14159265358979323846;
    . . .
}
```

If the keyword static had been omitted, then PI would have been an instance feld of the Math class. That is, you would need an object of this class to access PI, and every Math object would have its own copy of PI.



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
• When a method doesn’t need to access the object state because all needed parameters are supplied as explicit parameters (example: Math.pow).
• When a method only needs to access static felds of the class (example: Employee.getNextId).



### 4.6 Object Construction

**Overloading**

Some classes have more than one constructor. For example, you can construct an empty StringBuilder object as
`StringBuilder messages = new StringBuilder();`
Alternatively, you can specify an initial string:
`StringBuilder todoList = new StringBuilder("To do:\n"); `



**Default Field Initialization**

If you don’t set a field explicitly in a constructor, it is automatically set to a default value: numbers to 0, boolean values to false, and object references to null.



### 4.7 Packages

Java allows you to group classes in a collection called a package. Packages are convenient for organizing your work and for separating your work from code libraries provided by others. 



The main reason for using packages is to guarantee the uniqueness of class names. 



As long as both of them place their class into different packages, there is no conﬂict. In fact, to absolutely guarantee a unique package name, use an Internet domain name (which is known to be unique) written in reverse. 



**Class Importation**

A class can use all classes from its own package and all public classes from other packages. 



Locating classes in packages is an activity of the compiler. The bytecodes in class files always use full package names to refer to other classes. 



**Static Imports**

A form of the import statement permits the importing of static methods and fields, not just classes. 

```java
import static java.lang.System.*;

out.println("Goodbye, World!"); // i.e., System.out
exit(0); // i.e., System.exit
```



**Package Scope**

If you don’t specify either public or private, the feature (that is, the class, method, or variable) can be accessed by all methods in the same package. 



### 4.8 The Class Path

Class files can also be stored in a JAR (Java archive) file. A JAR file contains multiple class files and subdirectories in a compressed format, saving space and improving performance. 



### 4.9 Documentation Comments

**Class Comments**

```java
/**
* A {@code Card} object represents a playing card, such
* as "Queen of Hearts". A card has a suit (Diamond, Heart,
* Spade or Club) and a value (1 = Ace, 2 . . . 10, 11 = Jack,
* 12 = Queen, 13 = King)
*/
public class Card
{
    . . .
}
```



**Method Commnets**

```java
/**
* Raises the salary of an employee.
* @param byPercent the percentage by which to raise the salary (e.g. 10 means 10%)
* @return the amount of the raise
*/
public double raiseSalary(double byPercent)
{
    double raise = salary * byPercent / 100;
    salary += raise;
    return raise;
}
```



**Field Comments**

```java
/**
* The "Hearts" card suit
*/
public static final int HEARTS = 1;
```



### 4.10 Class Design Hints

- Always keep data private. 
- Always initialize data. 
- Don’t use too many basic types in a class. 
- Not all fields need individual field accessors and mutators. 
- Not all felds need individual feld accessors and mutators. 
- Make the names of your classes and methods reﬂect their responsibilities. 
- Prefer immutable classes 



## 5 Inheritance



### 5.1 Classes, Superclasses, and Subclasses

**Defining Subclasses**

- Here is how you defne a Manager class that inherits from the Employee class. Use the Java keyword extends to denote inheritance. 

```java
public class Manager extends Employee{
    added methods and fields
}
```

- The keyword extends indicates that you are making a new class that derives from an existing class. The existing class is called the superclass, base class, or parent class. The new class is called the subclass, derived class, or child class. 

- Subclasses have more functionality than their superclasses.  The Manager class encapsulates more data and has more functionality than its superclass Employee. 



**Overriding Methods**

- Some of the superclass methods are not appropriate for the Manager subclass. In particular, the getSalary method should return the sum of the base salary and the bonus. You need to supply a new method to override the superclass method: 

```java
public class Manager extends Employee{
    public double getSalary(){
        double baseSalary = super.getSalary();
        return baseSalary + bonus;
    }
}
```

- As you saw, a subclass can add felds, and it can add methods or override the methods of the superclass. However, inheritance can never take away any felds or methods. 



**Subclass Constructors**

- Here, the keyword super has a different meaning. The instruction is shorthand for “call the constructor of the Employee superclass with n, s, year, month, and day as parameters.” 

```java
public class Manager extends Employee{
    public Manager(String name, double salary, int year, int month, int day){
        super(name, salary, year, month, day);
        bonus = 0;
    }
}
```

- Since the Manager constructor cannot access the private felds of the Employee class, it must initialize them through a constructor. The constructor is invoked with the special super syntax. The call using super must be the frst statement in the constructor for the subclass. 

- If the subclass constructor does not call a superclass constructor explicitly, the no-argument constructor of the superclass is invoked. 
- If the superclass does not have a no-argument constructor and the subclass constructor does not call another superclass constructor explicitly, the Java compiler reports an error. 
- Recall that the this keyword has two meanings: to denote a reference to the implicit parameter and to call another constructor of the same class. Likewise, the super keyword has two meanings: to invoke a superclass method and to invoke a superclass constructor.  



**Inheritance Hierarchies**

- Inheritance need not stop at deriving one layer of classes. We could have an Executive class that extends Manager, for example. The collection of all classes extending a common superclass is called an inheritance hierarchy 
- The path from a particular class to its ancestors in the inheritance hierarchy is its inheritance chain. 

![Employee inheritance hierarchy](..\img\Employee inheritance hierarchy.png)



**Polymorphism**

- The “is–a” rule states that every object of the subclass is an object of the superclass. For example, every manager is an employee. Thus, it makes sense for the Manager class to be a subclass of the Employee class. Naturally, the opposite is not true—not every employee is a manager. 
- Another way of formulating the “is–a” rule is the substitution principle. That principle states that you can use a subclass object whenever the program expects a superclass object. 

```java
Employee e;
e = new Employee(. . .); // Employee object expected
e = new Manager(. . .); // OK, Manager can be used as well
```

- In the Java programming language, object variables are polymorphic. A variable of type Employee can refer to an object of type Employee or to an object of any subclass of the Employee class (such as Manager, Executive, Secretary, and so on). 
- In this case, the variables staff[0] and boss refer to the same object. However, staff[0] is considered to be only an Employee object by the compiler 

```java
Manager boss = new Manager(. . .);
Employee[] staff = new Employee[3];
staff[0] = boss;

boss.setBonus(5000); // OK
//The declared type of staff[0] is Employee, and the setBonus method is not a method of the Employee class.
staff[0].setBonus(5000); // Error
```

- However, you cannot assign a superclass reference to a subclass variable. The reason is clear: Not all employees are managers. 

```java
Manager m = staff[i]; // Error
```



**Understanding Method Calls**

1. The compiler looks at the declared type of the object and the method name. Note that there may be multiple methods, all with the same name, f, but with different parameter types. For example, there may be a method f(int) and a method f(String). The compiler enumerates all methods called f in the class C and all accessible methods called f in the superclasses of C. (Private methods of the superclass are not accessible.) Now the compiler knows all possible candidates for the method to be called.

2. Next, the compiler determines the types of the arguments that are supplied in the method call. If among all the methods called f there is a unique method whose parameter types are a best match for the supplied arguments, that method is chosen to be called. This process is called overloading resolution. For example, in a call x.f("Hello"), the compiler picks f(String) and not f(int). The situation can get complex because of type conversions (int to double, Manager to Employee, and so on). If the compiler cannot fnd any method with matching parameter types or if multiple methods all match after applying conversions, the compiler reports an error. Now the compiler knows the name and parameter types of the method that needs to be called. 

3. If the method is private, static, final, or a constructor, then the compiler knows exactly which method to call. (The final modifer is explained in the next section.) This is called static binding. Otherwise, the method to be called depends on the actual type of the implicit parameter, and dynamic binding must be used at runtime. In our example, the compiler would generate an instruction to call f(String) with dynamic binding. 

4. When the program runs and uses dynamic binding to call a method, the virtual machine must call the version of the method that is appropriate for the actual type of the object to which x refers. Let’s say the actual type is D, a subclass of C. If the class D defnes a method f(String), that method is called. If not, D’s superclass is searched for a method f(String), and so on.

- It would be time consuming to carry out this search every time a method is called. Therefore, the virtual machine precomputes for each class a method table that lists all method signatures and the actual methods to be called. When a method is actually called, the virtual machine simply makes a table lookup. In our example, the virtual machine consults the method table for the class D and looks up the method to call for f(String). That method may be D.f(String) or X.f(String), where X is some superclass of D. There is one twist to this scenario. If the call is super.f(param), then the compiler consults the method table of the superclass of the implicit parameter. 



- The getSalary method is not private, static, or final, so it is dynamically bound. The virtual machine produces method tables for the Employee and Manager classes. The Employeetable shows that all methods are defned in the Employee class itself:

```tab
Employee:
getName() -> Employee.getName()
getSalary() -> Employee.getSalary()
getHireDay() -> Employee.getHireDay()
raiseSalary(double) -> Employee.raiseSalary(double) 
```



- The Manager method table is slightly different. Three methods are inherited, one method is redefned, and one method is added. 

```
Manager:
getName() -> Employee.getName()
getSalary() -> Manager.getSalary()
getHireDay() -> Employee.getHireDay()
raiseSalary(double) -> Employee.raiseSalary(double)
setBonus(double) -> Manager.setBonus(double)
```



- Dynamic binding has a very important property: It makes programs extensible without the need for modifying existing code. Suppose a new class Executive is added and there is the possibility that the variable e refers to an object of that class. The code containing the call e.getSalary() need not be recompiled. The Executive.getSalary() method is called automatically if e happens to refer to an object of type Executive. 



- When you override a method, the subclass method must be at least as visible as the superclass method. In particular, if the superclass method is public, the subclass method must also be declared public. It is a common error to accidentally omit the public specifer for the subclass method. The compiler then complains that you try to supply a more restrictive access privilege. 



**Preventing Inheritance: Final Classes and Methods**

- Occasionally, you want to prevent someone from forming a subclass from one of your classes. Classes that cannot be extended are called fnal classes, and you use the final modifer in the defnition of the class to indicate this. 

```java
public final class Executive extends Manager{

}
```



- You can also make a specifc method in a class final. If you do this, then no subclass can override that method. (All methods in a final class are automatically final.) 

```java
public class Employee{
    public final String getName(){
        return name;
    }
}
```



- Recall that fields can also be declared as final. A final field cannot be changed after the object has been constructed. However, if a class is declared final, only the methods, not the fields, are automatically final. 



- Fortunately, the just-in-time compiler in the virtual machine can do a better job than a traditional compiler. It knows exactly which classes extend a given class, and it can check whether any class actually overrides a given method. If a method is short, frequently called, and not actually overridden, the just-in-time compiler can inline the method. 



**Casting**

- The process of forcing a conversion from one type to another is called casting. 

```java
double x = 3.405;
int nx = (int) x;
```



- The compiler checks that you do not promise too much when you store a value in a variable. If you assign a subclass reference to a superclass variable, you are promising less, and the compiler will simply let you do it. If you assign a superclass reference to a subclass variable, you are promising more. Then you must use a cast so that your promise can be checked at runtime 

```java
Manager boss = (Manager) staff[0];
```

- Thus, it is good programming practice to fnd out whether a cast will succeed before attempting it. 

```java
if (staff[1] instanceof Manager){
    boss = (Manager) staff[1];
}
```



- You can cast only within an inheritance hierarchy.
- Use instanceof to check before casting from a superclass to a subclass. 
- The only reason to make the cast is to use a method that is unique to managers, such as setBonus. 



**Abstract Classes**

- As you move up the inheritance hierarchy, classes become more general and probably more abstract. At some point, the ancestor class becomes so general that you think of it more as a basis for other classes than as a class with specifc instances you want to use. Consider, for example, an extension of our Employee class hierarchy. An employee is a person, and so is a student. Let us extend our class hierarchy to include classes Person and Student. 

- Why bother with so high a level of abstraction? There are some attributes that make sense for every person, such as name. Both students and employees have names, and introducing a common superclass lets us factor out the getName method to a higher level in the inheritance hierarchy 

![Inheritance diagram for Person and its subclasses](..\img\Inheritance diagram for Person and its subclasses.png)



- If you use the abstract keyword, you do not need to implement the method at all. 

```java
public abstract class Person{
    // no implementation required
    public abstract String getDescription();
}
```

- In addition to abstract methods, abstract classes can have felds and concrete methods.  

```java
public abstract class Person{
    private String name;
    public Person(String name){
        this.name = name;
    }
    public abstract String getDescription();
    public String getName(){
        return name;
    }
}
```



- Abstract methods act as placeholders for methods that are implemented in the subclasses. When you extend an abstract class, you have two choices. You can leave some or all of the abstract methods undefned; then you must tag the subclass as abstract as well. Or you can defne all methods, and the subclass is no longer abstract. 
- Abstract classes cannot be instantiated. That is, if a class is declared as abstract, no objects of that class can be created. 
- Note that you can still create object variables of an abstract class, but such a variable must refer to an object of a nonabstract subclass. For example: 

```java
Person p = new Student("Vince Vu", "Economics");
```

- Keep in mind that the variable p never refers to a Person object because it is impossible to construct an object of the abstract Person class. 

```
Person p = new Student(. . .);
p.getDescription()
```



**Protected Acess**

- As you know, felds in a class are best tagged as private, and methods are usually tagged as public. Any features declared private won’t be visible to other classes. 

- Protected methods make more sense. A class may declare a method as protected if it is tricky to use. This indicates that the subclasses (which, presumably, know their ancestor well) can be trusted to use the method correctly, but other classes cannot. 
- The four access modifers in Java that control visibility: 
  - Visible to the class only (private).
  - Visible to the world (public).
  - Visible to the package and all subclasses (protected).
  - Visible to the package—the (unfortunate) default. No modifers are needed. 



### 5.2 Object: The Cosmic Superclass

- The Object class is the ultimate ancestor—every class in Java extends Object. 
- In Java, only the values of primitive types (numbers, characters, and boolean values) are not objects. 

**The equals Method**

- The equals method in the Object class tests whether one object is considered equal to another. The equals method, as implemented in the Object class, determines whether two object references are identical. This is a pretty reasonable default—if two objects are identical, they should certainly be equal. 

```java
public class Employee{
    public boolean equals(Object otherObject){
        // a quick test to see if the objects are identical
        if (this == otherObject) return true;
        // must return false if the explicit parameter is null
        if (otherObject == null) return false;
        // if the classes don't match, they can't be equal
        if (getClass() != otherObject.getClass())
            return false;
        // now we know otherObject is a non-null Employee
        Employee other = (Employee) otherObject;
        // test whether the fields have identical values
        return Objects.equals(name, other.name)
            && salary == other.salary
            && Object.equals(hireDay, other.hireDay);
    }
}
```



**Equality Testing and Inheritance**

- The Java Language Specifcation requires that the equals method has the following properties: 
  - It is reﬂexive: For any non-null reference x, x.equals(x) should return true.
  - It is symmetric: For any references x and y, x.equals(y) should return true if and only if y.equals(x) returns true.
  - It is transitive: For any references x, y, and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true.
  - It is consistent: If the objects to which x and y refer haven’t changed, then repeated calls to x.equals(y) return the same value.
  - For any non-null reference x, x.equals(null) should return false. 



- Here is a recipe for writing the perfect equals method:
  - Name the explicit parameter otherObject—later, you will need to cast it to another variable that you should call other.

  - Test whether this happens to be identical to otherObject: 

    `if (this == otherObject) return true;`

    This statement is just an optimization. In practice, this is a common case. It is much cheaper to check for identity than to compare the felds.

  - Test whether otherObject is null and return false if it is. This test is required. 

    `if (otherObject == null) return false;`

  - Compare the classes of this and otherObject. If the semantics of equals can change in subclasses, use the getClass test: 

    `if (getClass() != otherObject.getClass()) return false;`

    If the same semantics holds for all subclasses, you can use an instanceof test: 

    `if (!(otherObject instanceof ClassName)) return false;`

  - Cast otherObject to a variable of your class type: 

    `ClassName other = (ClassName) otherObject`

  - Now compare the felds, as required by your notion of equality. Use == for primitive type felds, Objects.equals for object felds. Return true if all felds match, false otherwise.

    ```java
    return field1 == other.field1
    && Objects.equals(field2, other.field2)
        && . . .;
    ```

    If you redefne equals in a subclass, include a call to super.equals(other). 

**The hashCode Method**

**The toString Method**



5.3 Generic Array Lists

5.4 Object Wrappers and Autoboxing

5.5 Methods with a Variable Number of Parameters

5.6 Enmeration Classes

5.7 Reflection

5.8 Design Hints for Inheritance



