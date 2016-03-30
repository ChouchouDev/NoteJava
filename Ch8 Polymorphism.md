###多态
####8.1 再论向上转型
对象既可以作为它自己本身的类型使用，也可以作为它的基类型使用，而这种把对某个对象的引用视为对其基类型的引用的做法被称作向上转型
——因为在继承树的画法中，基类是放置在上方的
```java
Cercle c = new shape();
```
重写方法时，首先先考虑父类的方法。
####8.2 转机
注意：  
并非所有事物都可以多态发生，。然而，只有普通的方法调用是多态的。例如，如果你直接访问某个域，这个访问将在编译时进行解析，如下面这个例子
```java
//: polymorphism/FieldAccess.java
// Direct field access is determined at compile time.

class Super {
  public int field = 0;
  public int getField() { return field; }
}

class Sub extends Super {
  public int field = 1;
  public int getField() { return field; }
  public int getSuperField() { return super.field; }
}

public class FieldAccess {
  public static void main(String[] args) {
    Super sup = new Sub(); // Upcast
    System.out.println("sup.field = " + sup.field +
      ", sup.getField() = " + sup.getField());
      /* 当Sub对象转型为Super引用时，任何域访问操作都将由编译器解析，
因此不是多态的。在这里，为Super.field和Sub.field分配了不同的存储空间，这样，Sub实际上包含两个称为field的域：它自己的，它从Super
处获得的。为了得到Super.field，必须显式地指明super.field*/
    Sub sub = new Sub();
    System.out.println("sub.field = " +
      sub.field + ", sub.getField() = " +
      sub.getField() +
      ", sub.getSuperField() = " +
      sub.getSuperField());
  }
} /* Output:
sup.field = 0, sup.getField() = 1   

sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0
*///:~

```
####8.3 构造器和多态
顺序： 调用基类构造器，清理顺序则相反（dispose()）
注意：  
一个动态绑定的方法调用能由外深入到继承层次结构内部，它可以调用导出类里的方法，但是这个方法所操作的成员可能还未进行
初始化.如下：
```java
//: polymorphism/PolyConstructors.java
// Constructors and polymorphism
// don't produce what you might expect.
import static net.mindview.util.Print.*;

class Glyph {
  void draw() { print("Glyph.draw()"); }
  Glyph() {
    print("Glyph() before draw()");
    draw();     //此处调用时，得到的radius的初始化值是0，而不是1
    print("Glyph() after draw()");
  }
}	

class RoundGlyph extends Glyph {
  private int radius = 1;   
  RoundGlyph(int r) {
    radius = r;
    print("RoundGlyph.RoundGlyph(), radius = " + radius);
  }
  void draw() {
    print("RoundGlyph.draw(), radius = " + radius);
  }
}	

public class PolyConstructors {
  public static void main(String[] args) {
    new RoundGlyph(5);
  }
} /* Output:
Glyph() before draw()
RoundGlyph.draw(), radius = 0
Glyph() after draw()
RoundGlyph.RoundGlyph(), radius = 5
*///:~
```
####8.4 协变返回类型 ？？？
表示在导出类中的被覆盖方法可以返回基类方法的返回类型的某种导出类型
####8.5 用继承进行设计
#####8.5.1 纯继承和扩展
扩展（is-like-a）比纯继承（is-a）更加实用
#####8.5.2 向下转型与运行时类型识别

