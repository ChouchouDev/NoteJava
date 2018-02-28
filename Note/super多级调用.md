### Java 为什么不支持super多级调用，即super.super ?

#### 一、几种解释
* 1. 对于子类而言，已经继承了父类的所有，那么自然也继承了爷爷类的所有，使用super.super 属于多此一举。
* 2. super.super 属于多继承错误。
* 3. super是当前类的私有成员（或者说成是“隐含”的私有成员），代表着父类；而super.super的意思是要访问父类中的私有成员，所以不可能。或者换种说法，super和this其实都不是类的成员，根本不存在 **对象.成员** （即类似于：this.super,super.this）的语句。
* 4. super.super 从理论上是可行的。  
先从this说起，所有intance的方法在编译时都会被编译器多加入一个参数，就是this，这个this相当于一个指向当前对象本身的指针。运行时通过这个this，虚拟机就能找到当前对象在内存中的地址，从而得知当前对象的确切类型，从而实现多态。  
super的作用和this相似，编译器会把它作为一个参数加入instance的方法里。在虚拟机运行时就会去执行当前对象父类的方法。
那么我们以此类推，super.super其实是完全可以实现的。
Java之所以没有这种语句，完全是出于软件工程的考虑。
* 5. super.super 破坏了Java的封装性，对于子类而言，它只考虑它自己和父类，而不关心父类的父类

#### 二、本人思考
首先来解决我们是否会需要super.super的问题，也就是它是否多此一举，即解释1。  
答案是否定的。  
我们假设下面一种简单情况。 

> 图形 --> 长方形 --> 正方形  
> 为表示基本信息  
> 图形 toString返回：中心点（x,y）,颜色（R，G, B）, 压缩比, 偏移,...  
> 长方形 toString返回：super.toString() , 长, 宽

那现在正方形怎样的toString()更合适显示出它的基本信息呢？自然应该是：图形.toString() ,边长。而非长方形.toString。      
这里的图形.toString 在正方形类中正是super.super.toString()  

总结起来就是说： 假设爷爷有方法A1，父亲继承爷爷有了A1,但是重写成A2；之后儿子继承了父亲，拥有是A2，而并非A1。所以我们还是有可能需要用到祖父级的方法的。

  
那么super.super是否属于多继承错误呢？   
我们知道禁止多继承主要是为了防止冲突。但是我不认为在这里是合理解释，因为首先在建立类的时候，就决定了任何方法的标识都唯一指向一个方法：  
> （1）子类相对于父类新增的方法
> （2）子类继承于父类的方法
> （3）子类继承于父类，但是经过重写的方法。(父类方法被抛弃) 

同时对象调用方法时是以A.method()出现的，并没有A.super.method()，更不可能有A.super.super.method()，即使有，也已经完全区别开来了。所以不会有冲突产生！


解释3是说得通的，将this和super都看做一个“隐含”的私有成员（或者只是个标识）。私有性质限制了其他类的访问，或者"."的标识禁止了**对象.对象**这样的语法，也就是JDK规范的问题，this.super，super.this等也不被允许。

如果上述解释3是根本原因，那自然好过。而super.super历史上是存在的，http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4209952， 那么语法规范不太可能是根本原因。  

权威答案呢？不见得有了。解释4和5基本上可以统一说成是基于经济、实用性的考量。
> 1. 回到super多级调用需不需要的问题，子类其实继承的是父类以上辈分的方法，需要用super多级调用来追溯到更“原始”的方法往往是特殊情况,实用性不强。
> 2. super多级调用使得对象的封装性受到了威胁。 

总的来说就是super多级调用得不偿失！这是我最倾向的一个答案！  
这是很老的一个问题，希望有人知道更细致、权威的解释告诉一声。  
初学Java一个月，急需指导。

#### 三、替代super.super.xxx的方法实现

```java
class A {
    public void method() { }
}

class B extends A {
    public void method() { }
    protected void superMethod() {
         super.method();
    }
}

class C extends B {
    public void method() { }

    void test() {
        method();          // C.method()
        super.method();    // B.method()
        superMethod();     // A.method()
    }
}

```
