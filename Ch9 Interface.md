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
