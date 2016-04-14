###内部类
>转：内部类最大的优点就在于它能够非常好的解决多重继承的问题  
>1、内部类可以用多个实例，每个实例都有自己的状态信息，并且与其他外围对象的信息相互独立。  
>2、在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或者继承同一个类。  
>3、创建内部类对象的时刻并不依赖于外围类对象的创建。  
>4、内部类并没有令人迷惑的“is-a”关系，他就是一个独立的实体。  
>5、内部类提供了更好的封装，除了该外围类，其他类都不能访问。  

####10.1 创建内部类
```java
//: innerclasses/Parcel1.java
// Creating inner classes.

public class Parcel1 {
  class Contents {
    private int i = 11;
    public int value() { return i; }
  }
  public static void main(String[] args) {
    Parcel1 p = new Parcel1();
    p.ship("Tasmania");
  }
} /* Output:
Tasmania
*///:~
```

####10.2 链接外部类
所有的内部类自动拥有对其外围类所有成员的访问权。
####10.3 使用.this和.new
生成对外部类对象的引用

```java 
//: innerclasses/DotThis.java
// Qualifying access to the outer-class object.

public class DotThis {
  void f() { System.out.println("DotThis.f()"); }
  public class Inner {
    public DotThis outer() {
      return DotThis.this;
      // A plain "this" would be Inner's "this"
    }
  }
  public Inner inner() { return new Inner(); }
  public static void main(String[] args) {
    DotThis dt = new DotThis();
    DotThis.Inner dti = dt.inner();
    dti.outer().f();
  }
} /* Output:
DotThis.f()
*///:~
```

让某些对象，创建其某个内部类的对象
```java
//: innerclasses/DotNew.java
// Creating an inner class directly using the .new syntax.

public class DotNew {
  public class Inner {}
  public static void main(String[] args) {
    DotNew dn = new DotNew();
    DotNew.Inner dni = dn.new Inner();
  }
} ///:~
```

####10.4 内部类与向上转型
当将额你不累向上转型为基类，有期是转型为一个接口的时候，内部类就有了用武之处，。这是因为此内部类的——某个接口的实现可以完全不可见，并且
不可用。所得到的只是指向基类或接口的引用，所以能够很方便地隐藏实现细节。

####10.5 在方法和作用域内的内部类
如前所说的，都全部是成员内部类。
#####局部内部类
在方法的作用域内创建一个类,甚至可在任意的作用域内嵌入一个内部类

```java
//: innerclasses/Parcel6.java
// Nesting a class within a scope.

public class Parcel6 {
  private void internalTracking(boolean b) {
    if(b) {
      class TrackingSlip {
        private String id;
        TrackingSlip(String s) {
          id = s;
        }
        String getSlip() { return id; }
      }
      TrackingSlip ts = new TrackingSlip("slip");
      String s = ts.getSlip();
    }
    // Can't use it here! Out of scope:
    //! TrackingSlip ts = new TrackingSlip("x");
  }	
  public void track() { internalTracking(true); }
  public static void main(String[] args) {
    Parcel6 p = new Parcel6();
    p.track();
  }
} ///:~
```

####10.6 匿名内部类
#####注意：
>1、使用匿名内部类时，我们必须是继承一个类或者实现一个接口，但是两者不可兼得，同时也只能继承一个类或者实现一个接口。  
>2、匿名内部类中是不能定义构造函数的。  
>3、匿名内部类中不能存在任何的静态成员变量和静态方法。  
>4、匿名内部类为局部内部类，所以局部内部类的所有限制同样对匿名内部类生效。  
>5、匿名内部类不能是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方法。  
有下面这样一个类

```java
//: innerclasses/Parcel7b.java
// Expanded version of Parcel7.java

public class Parcel7b {
  class MyContents implements Contents {
    private int i = 11;
    public int value() { return i; }
  }
  public Contents contents() { return new MyContents(); }
  public static void main(String[] args) {
    Parcel7b p = new Parcel7b();
    Contents c = p.contents();
  }
} ///:~
```

使用匿名类后
```java
//: innerclasses/Parcel7.java
// Returning an instance of an anonymous inner class.

public class Parcel7 {
  public Contents contents() {
    return new Contents() { // Insert a class definition
      private int i = 11;
      public int value() { return i; }
    }; // Semicolon required in this case
  }
  public static void main(String[] args) {
    Parcel7 p = new Parcel7();
    Contents c = p.contents();
  }
} ///:~
```
匿名类中，参数引用必须为final, 不过如果在内部类中，参数不需要使用，就可以不用final
```java
//: innerclasses/Parcel9.java
// An anonymous inner class that performs
// initialization. A briefer version of Parcel5.java.

public class Parcel9 {
  // Argument must be final to use inside
  // anonymous inner class:
  public Destination destination(final String dest) {
    return new Destination() {
      private String label = dest;
      public String readLabel() { return label; }
    };
  }
  public static void main(String[] args) {
    Parcel9 p = new Parcel9();
    Destination d = p.destination("Tasmania");
  }
} ///:~
```
领略一下使用匿名内部类的魅力：  ????????? 不懂！
例1
```java
//: innerclasses/Factories.java
import static net.mindview.util.Print.*;

interface Service {
  void method1();
  void method2();
}

interface ServiceFactory {
  Service getService();
}	

class Implementation1 implements Service {
  private Implementation1() {}
  public void method1() {print("Implementation1 method1");}
  public void method2() {print("Implementation1 method2");}
  public static ServiceFactory factory =
    new ServiceFactory() {
      public Service getService() {
        return new Implementation1();
      }
    };
}	

class Implementation2 implements Service {
  private Implementation2() {}
  public void method1() {print("Implementation2 method1");}
  public void method2() {print("Implementation2 method2");}
  public static ServiceFactory factory =
    new ServiceFactory() {
      public Service getService() {
        return new Implementation2();
      }
    };
}	

public class Factories {
  public static void serviceConsumer(ServiceFactory fact) {
    Service s = fact.getService();
    s.method1();
    s.method2();
  }
  public static void main(String[] args) {
    serviceConsumer(Implementation1.factory);
    // Implementations are completely interchangeable:
    serviceConsumer(Implementation2.factory);
  }
} /* Output:
Implementation1 method1
Implementation1 method2
Implementation2 method1
Implementation2 method2
*///:~
```
例2
```java
//: innerclasses/Games.java
// Using anonymous inner classes with the Game framework.
import static net.mindview.util.Print.*;

interface Game { boolean move(); }
interface GameFactory { Game getGame(); }

class Checkers implements Game {
  private Checkers() {}
  private int moves = 0;
  private static final int MOVES = 3;
  public boolean move() {
    print("Checkers move " + moves);
    return ++moves != MOVES;
  }
  public static GameFactory factory = new GameFactory() {
    public Game getGame() { return new Checkers(); }
  };
}	

class Chess implements Game {
  private Chess() {}
  private int moves = 0;
  private static final int MOVES = 4;
  public boolean move() {
    print("Chess move " + moves);
    return ++moves != MOVES;
  }
  public static GameFactory factory = new GameFactory() {
    public Game getGame() { return new Chess(); }
  };
}	

public class Games {
  public static void playGame(GameFactory factory) {
    Game s = factory.getGame();
    while(s.move())
      ;
  }
  public static void main(String[] args) {
    playGame(Checkers.factory);
    playGame(Chess.factory);
  }
} /* Output:
Checkers move 0
Checkers move 1
Checkers move 2
Chess move 0
Chess move 1
Chess move 2
Chess move 3
*///:~
```
####10.7 嵌套类
如果不需要内部类对象与其外围类对象之间有联系，那么可以将内部类声明为static，这就是*嵌套类*
```java
//: innerclasses/Parcel11.java
// Nested classes (static inner classes).

public class Parcel11 {
  private static class ParcelContents implements Contents {
    private int i = 11;
    public int value() { return i; }
  }
  protected static class ParcelDestination
  implements Destination {
    private String label;
    private ParcelDestination(String whereTo) {
      label = whereTo;
    }
    public String readLabel() { return label; }	
    // Nested classes can contain other static elements:
    public static void f() {}
    static int x = 10;
    static class AnotherLevel {
      public static void f() {}
      static int x = 10;
    }
  }
  public static Destination destination(String s) {
    return new ParcelDestination(s);
  }
  public static Contents contents() {
    return new ParcelContents();
  }
  public static void main(String[] args) {
    Contents c = contents();
    Destination d = destination("Tasmania");
  }
} ///:~

```
注意:  
1、正常情况下，接口内部不能防止代码，但嵌套类可以作为接口的一部分。放到接口中的任何类都自动是public和static的。   
2、一个内部类被嵌套多少层并不重要

####10.8 为什么需要内部类
最吸引人的方面在于： 每个内部类都能独立地继承自一个（接口的）实现，所以无论外围类是否已经继承了某个（接口的）实现，对于内部类都没有影响。  
#####特性：
1）内部类可以有多个实例，每个实例都有自己的状态信息，并且与其外围类对象的信息相互独立  
2）在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或继承同一个类。  
3）创建内部类对象的时刻并不依赖于外围类对象的创建  
4）内部类并没有令人迷惑的“is-a“关系，它就是一个独立的实体  

####10.8.1 闭包和回调   ？？？？


