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
####7.8 final数据


 
