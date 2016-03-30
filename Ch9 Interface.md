###接口
####9.1 抽象类和抽象方法
如果一个类包括一个或多个抽象方法，该类必须被限定为抽象的
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
