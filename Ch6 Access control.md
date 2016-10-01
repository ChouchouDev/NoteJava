###访问权限控制
####6.1 包：库单元
```java
pakage net.mindview.simple //用域名得到唯一的库名
import java.util.ArrayList;
import java.util.*;
```
####6.2 Java访问权限修饰词
6.2.1 包访问权限
注意：  
当一个类或类中的方法没有权限修饰词时，默认为包访问权限  
6.2.2 public:接口访问权限  
6.2.3 private:无法访问  
6.2.4 继承访问权限  
对于类成员（字段和方法）的访问权限来说，  
public：所有类都可访问。  
protected：继承访问权限。基类通过protected把访问权限赋予派生类而不是所有类，  
另外，protected也提供包访问权限，也就是说，相同包内的其他类可以访问protected元素。  
private：除了包含这个成员的类外，其他任何类都无法访问这个成员。  
####6.3 接口与实现
访问权限的控制常被称为具体实现的隐藏。把数据和方法包装进类里，以及具体实现的隐藏，常共同被称作*封装*
####6.4 类的访问权限
注意:  
1、每个编译单元（文件）都只能有一个public类  
2、public类的名称必须和文件名相匹配（编译单元内完全不带public类也是可能的，这时可以随意对文件命名）  
3、类既不可以是private的（这样会使得除该类之外，其他任何类都不可以访问它），也不可以是protected的。（事实上，一个内部类可以是
private或是protected的，但那是特例，将在第10章介绍）  
如果不希望任何其他人对该类有访问权限，可以把所有的构造器都指定为private，从而阻止任何人创建该类的对象，但是有一个例外，
就是你在该类的static的成员内部可以创建。  
  
  
以下展示了如何通过将所有的构造器指定为private阻止直接某个类的实例，我们给出了两种使用该该种类的方法。
soup1，创建了一个static方法  
soup2，用到了所谓的设计模式，被称为singleton，这是因为你始终只能创建它的一个对象。soup2类的对象是作为soup2的一个static private
成员创建的，所以有且仅有一个，而且除非通过public方法access()，否则无法访问到它。  
```java
//: access/Lunch.java
// Demonstrates class access specifiers. Make a class
// effectively private with private constructors:

class Soup1 {
  private Soup1() {}
  // (1) Allow creation via static method:
  public static Soup1 makeSoup() {
    return new Soup1();
  }
}

class Soup2 {
  private Soup2() {}
  // (2) Create a static object and return a reference
  // upon request.(The "Singleton" pattern):
  private static Soup2 ps1 = new Soup2();
  public static Soup2 access() {
    return ps1;
  }
  public void f() {}
}

// Only one public class allowed per file:
public class Lunch {
  void testPrivate() {
    // Can't do this! Private constructor:
    //! Soup1 soup = new Soup1();
  }
  void testStatic() {
    Soup1 soup = Soup1.makeSoup();
  }
  void testSingleton() {
    Soup2.access().f();
  }
} ///:~
```

