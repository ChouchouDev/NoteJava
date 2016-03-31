###接口
####9.1 抽象类和抽象方法
如果一个类包括一个或多个抽象方法，该类必须被限定为抽象的
```java
abstract class Demo｛
abstract void method1();
abstract void method2();
…
｝
```
区别（来自网上）：  
>1.abstract class 在 Java 语言中表示的是一种继承关系，一个类只能使用一次继承关系。但是，一个类却可以实现多个interface。  
>2.在abstract class 中可以有自己的数据成员，也可以有非abstarct的成员方法，而在interface中，只能够有静态的不能被修改的数据成员（也就是必须是static final的，不过在 interface中一般不定义数据成员），所有的成员方法都是abstract的。  
>3.abstract class和interface所反映出的设计理念不同。其实abstract class表示的是"is-a"关系，interface表示的是"like-a"关系。  
>4.实现抽象类和接口的类必须实现其中的所有方法。抽象类中可以有非抽象方法。接口中则不能有实现方法。  
>5.接口中定义的变量默认是public static final 型，且必须给其初值，所以实现类中不能重新定义，也不能改变其值。  
>6.抽象类中的变量默认是 friendly 型，其值可以在子类中重新定义，也可以重新赋值。   
>7.接口中的方法默认都是 public,abstract 类型的。
####9.2 接口
接口是java语言中对抽象概念进行定义的另一种机制。想要创建一个接口，需要用interface关键字来
代替class关键字。如果不添加public关键字，则它只有包访问权限。接口也可以包括域，但是这些
域隐式地是static和final的。  
方法都必须是public的。
```java
interface Instrument {
  // Compile-time constant:
  int VALUE = 5; // static & final
  // Cannot have method definitions:
  void play(Note n); // Automatically public
  void adjust();
}

class Wind implements Instrument {
  public void play(Note n) {
    print(this + ".play() " + n);
  }
  public String toString() { return "Wind"; }
  public void adjust() { print(this + ".adjust()"); }
}
```
####9.3 完全解耦 ？？？
完全解除类与类的依赖关系。  
只要一个方法操作的是类而非接口，那么我们就只能使用这个类和其子类。而且我们不能将这个方法应用于不在此继承结构中的某个类。
####9.4 Java中的多重继承
使用接口的核心原因：  
1、为了能够向上转型为多个基本类型（以及由此带来的灵活性）  
2、与使用抽象基类相同，接口可防止客户端程序员创建该类的对象，并确保这仅仅是建立一个接口
```java
//: interfaces/Adventure.java
// Multiple interfaces.

interface CanFight {
  void fight();
}

interface CanSwim {
  void swim();
}

interface CanFly {
  void fly();
}

class ActionCharacter {
  public void fight() {}
}	

class Hero extends ActionCharacter implements CanFight, CanSwim, CanFly {
  public void swim() {}
  public void fly() {}
}

public class Adventure {
  public static void t(CanFight x) { x.fight(); }
  public static void u(CanSwim x) { x.swim(); }
  public static void v(CanFly x) { x.fly(); }
  public static void w(ActionCharacter x) { x.fight(); }
  public static void main(String[] args) {
    Hero h = new Hero();
    t(h); // Treat it as a CanFight
    u(h); // Treat it as a CanSwim
    v(h); // Treat it as a CanFly
    w(h); // Treat it as an ActionCharacter
  }
} ///:~
```
####9.5 通过继承来扩展接口
```java
//: interfaces/HorrorShow.java
// Extending an interface with inheritance.

interface Monster {
  void menace();
}

interface DangerousMonster extends Monster {
  void destroy();
}

interface Lethal {
  void kill();
}

class DragonZilla implements DangerousMonster {
  public void menace() {}
  public void destroy() {}
}	

interface Vampire extends DangerousMonster, Lethal {
  void drinkBlood();
}

class VeryBadVampire implements Vampire {
  public void menace() {}
  public void destroy() {}
  public void kill() {}
  public void drinkBlood() {}
}	

public class HorrorShow {
  static void u(Monster b) { b.menace(); }
  static void v(DangerousMonster d) {
    d.menace();
    d.destroy();
  }
  static void w(Lethal l) { l.kill(); }
  public static void main(String[] args) {
    DangerousMonster barney = new DragonZilla();
    u(barney);
    v(barney);
    Vampire vlad = new VeryBadVampire();
    u(vlad);
    v(vlad);
    w(vlad);
  }
} ///:~
```
注意多重继承时方法之间的可区分性：  
int f();     
int f(int a);  
void f();  
第一和第三不可分。
####9.6 适配接口 ？？？
接口最吸引人的地方就是允许同一个接口具有多个不同的实现。“你可以用任何你想要的对象来调用我的方法，只要你的对象遵循我的接口”
Java SE5d的scanner类 （第13章）
####9.7 接口中的域
放进接口中的任何域都自动是static和final的
```java
interface Days {
	int SUNDAY = 1, MONDAY = 2, TUESDAY = 3, WEDNESDAY = 4, 
		THURSDAY = 5, FRIDAY = 6, SATURDAY = 7; 
}

class Week implements Days {
	private static int count = 0;
	private int id = count++;
	public Week() { System.out.println("Week() " + id); }
}

public class Ex17 {
	public static void main(String[] args) {
		// Without any objects, static fields exist:
		System.out.println("SUNDAY = " + Days.SUNDAY);
		System.out.println("MONDAY = " + Days.MONDAY);
		Week w0 = new Week();
		Week w1 = new Week();
		// Error: cannot assign a value to final variable SUNDAY:
		// w.SUNDAY = 2;
		// Error: cannot assign a value to final variable MONDAY: 
		// w1.MONDAY = w0.MONDAY;
	}
}
```
####9.8 嵌套接口
一般接口都不能设为private或protected,而在嵌套时，作为一种添加的方法，可以设为private或protected
```java
public class Test {
  
   private interface Sortable {
   }
  
   protected interface Searchable {
   }
  
}
```
####9.9 接口与工厂
接口是实现多重继承的途径，而生成遵循某个接口的对象的典型方法就是*工厂方法*设计模型。它与直接调用构造器不同，我们在工厂对象上调用的创建方法，而该工厂对象将生成接口的某个实现的对象。
```java
//: interfaces/Games.java
// A Game framework using Factory Methods.
import static net.mindview.util.Print.*;

interface Game { boolean move(); }
interface GameFactory { Game getGame(); }

class Checkers implements Game {   //西洋跳棋 使用Game接口
  private int moves = 0;
  private static final int MOVES = 3;
  public boolean move() {
    print("Checkers move " + moves);
    return ++moves != MOVES;
  }
}

class CheckersFactory implements GameFactory {   //西洋跳棋工厂使用 总工厂接口
  public Game getGame() { return new Checkers(); }
}	

class Chess implements Game {       //国际象棋 使用Game接口
  private int moves = 0;
  private static final int MOVES = 4;
  public boolean move() {
    print("Chess move " + moves);
    return ++moves != MOVES;
  }
}

class ChessFactory implements GameFactory {
  public Game getGame() { return new Chess(); }
}	

public class Games {                //国际象棋工厂使用 总工厂接口
  public static void playGame(GameFactory factory) {
    Game s = factory.getGame();
    while(s.move())
      ;
  }
  public static void main(String[] args) {
    playGame(new CheckersFactory());
    playGame(new ChessFactory());
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
