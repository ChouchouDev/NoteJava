###复用类
复用方法：  
1、在新的类中产生现有类的对象  
2、按照现有类的类型来创建类 
####7.1 组合语法
注意：  
类中域为基本类型能够自动被初始化为零，但是对象引用会被初始化为null。  
如果想初始化这些引用，可以在代码中的下列位置进行：  
1、在定义对象的地方  
2、在类的构造器中  
3、就在正要使用这些对象之前，称为惰性初始化Delayed initialization
####7.2 继承语法
继承关系下的初始化过程
```java
//: reusing/Cartoon.java
// Constructor calls during inheritance.
import static net.mindview.util.Print.*;

class Art {
  Art() { print("Art constructor"); }
}

class Drawing extends Art {
  Drawing() { print("Drawing constructor"); }
}

public class Cartoon extends Drawing {
  public Cartoon() { print("Cartoon constructor"); }
  public static void main(String[] args) {
    Cartoon x = new Cartoon();
  }
} /* Output:
Art constructor
Drawing constructor
Cartoon constructor
*///:~
```
####7.3 代理
组合形式，然后方法书写如下：  
```java
public void up()
{
control.up();  // control为复用的类的一个对象
}
```
####7.4 结合使用组合和继承
####7.5 在组合与继承之间选择
####7.6 protected关键字
####7.7 向上转型
父类程序对它的所有的导出类都起作用，将子类引用转换为父类引用的动作，我们称之为向上转型。向下转型也有，将在第8章和第14章中
进一步解释
####7.8 final关键字
#####7.8.1 final 数据   
对于基本类型，final使得数字恒定不变；而用于对象引用，final使引用恒定不变。一旦引用被初始化指向一个对象，就无法再把它
改为指向另一个对象。然而对象本身是可以被修改的，Java并未提供使任何对象恒定不变的途径。
注意：  
1、带有恒定初始值（即，编译时常量）的final static基本类型全用大写字母命名，并且字与子之间下划线隔开。
2、空白final：必须在域的定义处或者构造器中用表达式对final进行赋值。
```java
//: reusing/BlankFinal.java
// "Blank" final fields.

class Poppet {
  private int i;
  Poppet(int ii) { i = ii; }
}

public class BlankFinal {
  private final int i = 0; // Initialized final
  private final int j; // Blank final
  private final Poppet p; // Blank final reference
  // Blank finals MUST be initialized in the constructor:
  public BlankFinal() {
    j = 1; // Initialize blank final
    p = new Poppet(1); // Initialize blank final reference
  }
  public BlankFinal(int x) {
    j = x; // Initialize blank final
    p = new Poppet(x); // Initialize blank final reference
  }
  public static void main(String[] args) {
    new BlankFinal();
    new BlankFinal(47);
  }
} ///:~
```
#####7.8.2 final方法 
把方法锁定；提高效率。  
类中所有的private方法都隐式地指定是final的。由于无法取用private方法，也就无法覆盖它。可以对private方法添加final修饰词，
但是这并不能有任何意义。  
#####7.8.3 final类
意味着无法被继承。
####7.9 初始化及类的加载
初始化过程，在Beetle上运行时，找到main入口，并且按顺序把static方法全部初始化一遍
```java
//: reusing/Beetle.java
// The full process of initialization.
import static net.mindview.util.Print.*;

class Insect {
  private int i = 9;
  protected int j;
  Insect() {
    print("i = " + i + ", j = " + j);
    j = 39;
  }
  private static int x1 =
    printInit("static Insect.x1 initialized");
  static int printInit(String s) {
    print(s);
    return 47;
  }
}

public class Beetle extends Insect {
  private int k = printInit("Beetle.k initialized");
  public Beetle() {
    print("k = " + k);
    print("j = " + j);
  }
  private static int x2 =
    printInit("static Beetle.x2 initialized");
  public static void main(String[] args) {
    print("Beetle constructor");
    Beetle b = new Beetle();
  }
} /* Output:
static Insect.x1 initialized
static Beetle.x2 initialized
Beetle constructor
i = 9, j = 0
Beetle.k initialized
k = 47
j = 39
*///:~

```





 
