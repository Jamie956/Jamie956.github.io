### String

|          | String                                                       | StringBuilder                                                | StringBuffer |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------ |
| 可变     | N                                                            | Y                                                            | Y            |
| 线程安全 | Y                                                            | N                                                            | Y(同步锁)    |
|          | 创建String时，如果向量池如果有相等的值，就不创建对象，而是引用那个值 | 比使用 StringBuffer 获得 10%~15% 左右的性能提升，但有线程安全问题 |              |
| 应用场景 | 操作少量数据                                                 | 单线程                                                       | 多线程       |

### Object-Oriented Programming

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

- Dependence (“uses–a”)

For example, the Order class uses the Account class because Order objects need to access Account objects to check for credit status. But the Item class does not depend on the Account class, because Item objects never need to worry about customer accounts. Thus, a class depends on another class if its methods use or manipulate objects of that class. 

- Aggregation (“has–a”)

The aggregation, or “has–a” relationship, is easy to understand because it is concrete; for example, an Order object contains Item objects. Containment means that objects of class A contain objects of class B.

- Inheritance (“is–a”) 

The inheritance, or “is–a” relationship, expresses a relationship between a more special and a more general class. For example, a RushOrder class inherits from an Order class. The specialized RushOrder class has special methods for priority handling and a different method for computing shipping charges, but its other methods, such as adding items and billing, are inherited from the Order class. In general, if class A extends class B, class A inherits methods from class B but has more capabilities.



![UML notation for class relationships](..\img\UML notation for class relationships.png)



### Construct

- To work with objects, you first construct them and specify their initial state. Then you apply methods to the objects.

- In the Java programming language, you use constructors to construct new instances.

- A constructor is a special method whose purpose is to construct and initialize objects.

- A constructor has the same name as the class.
- A class can have more than one constructor.
- A constructor can take zero, one, or more parameters.
- A constructor has no return value.
- A constructor is always called with the new operator. 



**Final Instance Fields**

You can define an instance field as final. Such a field must be initialized when the object is constructed. That is, you must guarantee that the field value has been set after the end of every constructor. Afterwards, the field may not be modified again.

```java
class Employee
{
    private final String name;
    . . .
}
```



### Static Field and Methods

**Static Fields**

```java
class Employee
{
    private static int nextId = 1;
    private int id;
    . . .
}
```

Every employee object now has its own id field, but there is only one nextId field that is shared among all instances of the class. Let’s put it another way. If there are 1,000 objects of the Employee class, then there are 1,000 instance fields id, one for each object. But there is a single static field nextId. Even if there are no employee objects, the static field nextId is present. It belongs to the class, not to any individual object.



**Static Constants**

```java
public class Math
{
    . . .
        public static final double PI = 3.14159265358979323846;
    . . .
}
```

If the keyword static had been omitted, then PI would have been an instance field of the Math class. That is, you would need an object of this class to access PI, and every Math object would have its own copy of PI.



**Static Methods**

You can think of static methods as methods that don’t have a this parameter.

A static method of the Employee class cannot access the id instance field because it does not operate on an object. However, a static method can access a static field.

```java
public static int getNextId()
{
    return nextId; // returns static field
}
```

Use static methods in two situations:
- When a method doesn’t need to access the object state because all needed parameters are supplied as explicit parameters (example: Math.pow).
- When a method only needs to access static fields of the class (example: Employee.getNextId).



### Object Construction

**Overloading**

Some classes have more than one constructor. For example, you can construct an empty StringBuilder object as
`StringBuilder messages = new StringBuilder();`
Alternatively, you can specify an initial string:
`StringBuilder todoList = new StringBuilder("To do:\n"); `



**Default Field Initialization**

If you don’t set a field explicitly in a constructor, it is automatically set to a default value: numbers to 0, boolean values to false, and object references to null.



### Packages

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



### Class Design Hints

- Always keep data private. 
- Always initialize data. 
- Don’t use too many basic types in a class. 
- Not all fields need individual field accessors and mutators. 
- Make the names of your classes and methods reﬂect their responsibilities. 
- Prefer immutable classes



###  Classes, Superclasses, and Subclasses

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

- As you saw, a subclass can add fields, and it can add methods or override the methods of the superclass. However, inheritance can never take away any fields or methods. 



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

- Since the Manager constructor cannot access the private fields of the Employee class, it must initialize them through a constructor. The constructor is invoked with the special super syntax. The call using super must be the frst statement in the constructor for the subclass. 

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

    ```
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

- In addition to abstract methods, abstract classes can have fields and concrete methods.  

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

    ```java
    Person p = new Student(. . .);
    p.getDescription()
    ```



**Protected Acess**

- As you know, fields in a class are best tagged as private, and methods are usually tagged as public. Any features declared private won’t be visible to other classes. 

- Protected methods make more sense. A class may declare a method as protected if it is tricky to use. This indicates that the subclasses (which, presumably, know their ancestor well) can be trusted to use the method correctly, but other classes cannot. 
- The four access modifers in Java that control visibility: 
  - Visible to the class only (private).
  - Visible to the world (public).
  - Visible to the package and all subclasses (protected).
  - Visible to the package—the (unfortunate) default. No modifers are needed. 



### Object: The Cosmic Superclass

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

    This statement is just an optimization. In practice, this is a common case. It is much cheaper to check for identity than to compare the fields.

  - Test whether otherObject is null and return false if it is. This test is required. 

    `if (otherObject == null) return false;`

  - Compare the classes of this and otherObject. If the semantics of equals can change in subclasses, use the getClass test: 

    `if (getClass() != otherObject.getClass()) return false;`

    If the same semantics holds for all subclasses, you can use an instanceof test: 

    `if (!(otherObject instanceof ClassName)) return false;`

  - Cast otherObject to a variable of your class type: 

    `ClassName other = (ClassName) otherObject`

  - Now compare the fields, as required by your notion of equality. Use == for primitive type fields, Objects.equals for object fields. Return true if all fields match, false otherwise.

    ```java
    return field1 == other.field1
    && Objects.equals(field2, other.field2)
        && . . .;
    ```

    If you redefne equals in a subclass, include a call to super.equals(other). 

**The hashCode Method**

- A hash code is an integer that is derived from an object. 

- The hashCode method is defned in the Object class. Therefore, every object has a default hash code. That hash code is derived from the object’s memory address.

- The strings s and t have the same hash code because, for strings, the hash codes are derived from their contents. The string builders sb and tb have different hash codes because no hashCode method has been defned for the StringBuilder class and the default hashCode method in the Object class derives the hash code from the object’s memory address. 

  ```java
  String s = "Ok";
  StringBuilder sb = new StringBuilder(s);
  System.out.println(s.hashCode() + " " + sb.hashCode());
  String t = new String("Ok");
  StringBuilder tb = new StringBuilder(t);
  System.out.println(t.hashCode() + " " + tb.hashCode());
  ```

  ![Hash Codes of Strings and String Builders](..\img\Hash Codes of Strings and String Builders.png)



**The toString Method**

- Another important method in Object is the toString method that returns a string representing the value of this object.  

- Whenever an object is concatenated with a string by the “+” operator, the Java compiler automatically invokes the toString method to obtain a string representation of the object. 

  ```java
  Point p = new Point(10, 20);
  String message = "The current position is " + p;
  // automatically invokes p.toString()
  ```



### Generic Array Lists

- The ArrayList class is similar to an array, but it automatically adjusts its capacity as you add and remove elements, without any additional code. 

- ArrayList is a generic class with a type parameter. To specify the type of the element objects that the array list holds, you append a class name enclosed in angle brackets, such as `ArrayList<Employee>`

- Here we declare and construct an array list that holds Employee objects: 

  ```java
  ArrayList<Employee> staff = new ArrayList<Employee>();
  //Java7 - omit the type parameter on the right-hand side
  ArrayList<Employee> staff = new ArrayList<>();
  ```

- If you call add and the internal array is full, the array list automatically creates a bigger array and copies all the objects from the smaller to the bigger array. 

- If you already know, or have a good guess, how many elements you want to store, call the ensureCapacity method before flling the array list:
  `staff.ensureCapacity(100); `

- You can also pass an initial capacity to the ArrayList constructor: 

  `ArrayList<Employee> staff = new ArrayList<>(100); `



**Accessing Array List Elements**

Employee[] array is replaced by an `ArrayList<Employee>`. Note the following changes: 

- You don’t have to specify the array size.
- You use add to add as many elements as you like.
- You use size() instead of length to count the number of elements.
- You use a.get(i) instead of a[i] to access an element.



### Object Wrappers and Autoboxing

- A class Integer corresponds to the primitive type int. These kinds of classes are usually called wrappers.  
- The wrapper classes have obvious names: Integer, Long, Float, Double, Short, Byte, Character, and Boolean. 
- The wrapper classes are immutable—you cannot change a wrapped value after the wrapper has been constructed. They are also final, so you cannot subclass them. 
- autoboxing

    ```java
    ArrayList<Integer> list = new ArrayList<>();
    list.add(3);
    //automatically translated to
    list.add(Integer.valueOf(3));
    ```

- unboxing

  ```java
  int n = list.get(i);
  // the compiler translates into
  int n = list.get(i).intValue();
  ```

- Boxing and unboxing is a courtesy of the compiler, not the virtual machine. The compiler inserts the necessary calls when it generates the bytecodes of a class. The virtual machine simply executes those bytecodes. 



### Reflection

- The reﬂection library gives you a very rich and elaborate toolset to write programsthat manipulate Java code dynamically 

- A program that can analyze the capabilities of classes is called reﬂective. The reﬂection mechanism is extremely powerful. As the next sections show, you can use it to
  - Analyze the capabilities of classes at runtime;
  - Inspect objects at runtime—for example, to write a single toString method that works for all classes;
  - Implement generic array manipulation code; and
  - Take advantage of Method objects that work just like function pointers in languages such as C++. 



**The Class Class**

- While your program is running, the Java runtime system always maintains what is called runtime type identifcation on all objects. This information keeps track of the class to which each object belongs. Runtime type information is used by the virtual machine to select the correct methods to execute. 

- The getClass() method in the Object class returns an instance of Class type. 

  ```java
  Employee e;
  . . .
  Class cl = e.getClass();
  ```



**A Primer on Catching Exceptions**

- There are two kinds of exceptions: unchecked exceptions and checked exceptions. With checked exceptions, the compiler checks that you provide a handler. However, many common exceptions, such as accessing a null reference, are unchecked. The compiler does not check whether you provided a handler for these errors—after all, you should spend your mental energy on avoiding these mistakes rather than coding handlers for them. 

- But not all errors are avoidable. If an exception can occur despite your best efforts, then the compiler insists that you provide a handler. The Class.forName method is an example of a method that throws a checked exception. 

- Place one or more statements that might throw checked exceptions inside a try block. Then provide the handler code in the catch clause. 

  ```java
  try{
      String name = . . .; // get class name
      Class cl = Class.forName(name); // might throw exception
      do something with cl
  }
  catch (Exception e){
      e.printStackTrace();
  }
  ```

- If the class name doesn’t exist, the remainder of the code in the try block is skipped and the program enters the catch clause. (Here, we print a stack trace by using the printStackTrace method of the Throwable class. Throwable is the superclass of the Exception class.) If none of the methods in the try block throws an exception, the handler code in the catch clause is skipped 



**Using Reﬂection to Analyze Objects at Runtime**

- The default behavior of the reﬂection mechanism is to respect Java access control. However, if a Java program is not controlled by a security manager that disallows it, you can override access control. To do this, invoke the setAccessible method on a Field, Method, or Constructor object. For example: `f.setAccessible(true); // now OK to call f.get(harry); `



### Interfaces



**The Interface Concept**

- This means that any class that implements the Comparable interface is required to have a compareTo method, and the method must take an Object parameter and return an integer. 

  ```java
  public interface Comparable{
      int compareTo(Object other);
  }
  ```

- All methods of an interface are automatically public. For that reason, it is not necessary to supply the keyword public when declaring a method in an interface. 

- The reason for interfaces is that the Java programming language is strongly typed. When making a method call, the compiler needs to be able to check that the method actually exists. 



**Properties of Interfaces**

- Interfaces are not classes. In particular, you can never use the new operator to instantiate an interface

  `x = new Comparable(. . .); // ERROR `

- An interface variable must refer to an object of a class that implements the interface 

- Next, just as you use instanceof to check whether an object is of a specifc class, you can use instanceof to check whether an object implements an interface: 

  `if (anObject instanceof Comparable) { . . . } `

- Although you cannot put instance fields or static methods in an interface, you can supply constants in them. For example: 

  ```java
  public interface Powered extends Moveable{
      double milesPerGallon();
      double SPEED_LIMIT = 95; // a public static final constant
  }
  ```

  Just as methods in an interface are automatically public, fields are always public static final. 

**Interfaces and Abstract Classes**

- There is, unfortunately, a major problem with using an abstract base class to express a generic property. A class can only extend a single class. Suppose the Employee class already extends a different class, say, Person. Then it can’t extend a second class. 

  `class Employee extends Person, Comparable // Error `


