#### 3.1 更简单的打印语句
#### 3.2 使用java操作符
#### 3.3 优先级
#### 3.4 赋值
注意：  
对象赋值时：将“引用”从一个地方复制到另外一个地方，别名问题
#### 3.5 算数操作符
```java
// Create a seeded random number generator:
    Random rand = new Random(47);
    int i, j, k;
    // Choose value from 1 to 100:
    j = rand.nextInt(100) + 1;
```
java.util.Random类中实现的随机算法是伪随机，也就是有规则的随机，所谓有规则的就是在给定种(seed)的区间内随机生成数字；
种子数只是随机算法的起源数字，和生成的随机数的区间没有任何关系.

java.Math.Random()实际是在内部调用java.util.Random()的,它有一个致命的弱点，它和系统时间有关.
#### 3.6 自动递增和递减
#### 3.7 关系操作符
测试对象的等价性
注意：
```java
public class Equivalence {
  public static void main(String[] args) {
    Integer n1 = new Integer(47);
    Integer n2 = new Integer(47);
    System.out.println(n1 == n2);
    System.out.println(n1 != n2);
  }
} /* Output:
false
true
*///:~
```
上述的==和!=比较的是对象的引用  
可以使用 A.equals(B) 来比较内容， 注意equals()默认的是比较引用，所以需要在类中覆盖equals()方法。
#### 3.8 逻辑操作符
注意：  
1、与在C和C++中不同的是，不可将一个非布尔值当做布尔值在逻辑表达式中使用  
例： print("i && j is "+ (i && j));   
2、短路问题
一旦能够明确无误地确定整个表达式的值，就不再计算表达式的余下部分  
例： boolean b = test1(0) && test2 (1) && test3 (2)
#### 3.9 直接常量
```java
    int i1 = 0x2f; // Hexadecimal (lowercase)
    int i2 = 0X2F; // Hexadecimal (uppercase)
    int i3 = 0177; // Octal (leading zero)
    char c = 0xffff; // max char hex value
    byte b = 0x7f; // max byte hex value
    short s = 0x7fff; // max short hex value
    long n1 = 200L; // long suffix
    long n2 = 200l; // long suffix (but can be confusing)
    long n3 = 200;
    float f1 = 1;
    float f2 = 1F; // float suffix
    float f3 = 1f; // float suffix
    double d1 = 1d; // double suffix
    double d2 = 1D; // double suffix
    
    print("b: " + Integer.toBinaryString(b));//Integer和Long类 的静态方法
    
    float expFloat = 1.39e-43f // 打印出来为：1.39E-43 , E为10次幂
```
#### 3.10 按位操作符
#### 3.11 移位操作符
注意：  
如果对byte（8位）或short（16位）值进行右移操作，他们会先被转换为int类型，再进行右移操作，然后被截断，复制给原来的类型，在这种情况下可能得到-1的结果  
```java 
import static net.mindview.util.Print.*;

public class URShift {
  public static void main(String[] args) {
    int i = -1;
    print(Integer.toBinaryString(i));
    i >>>= 10;
    print(Integer.toBinaryString(i));
    long l = -1;
    print(Long.toBinaryString(l));
    l >>>= 10;
    print(Long.toBinaryString(l));
    short s = -1;
    print(Integer.toBinaryString(s));
    s >>>= 10;
    print(Integer.toBinaryString(s));
    byte b = -1;
    print(Integer.toBinaryString(b));
    b >>>= 10;
    print(Integer.toBinaryString(b));
    b = -1;
    print(Integer.toBinaryString(b));
    print(Integer.toBinaryString(b>>>10));
  }
} /* Output:
11111111111111111111111111111111
1111111111111111111111
1111111111111111111111111111111111111111111111111111111111111111
111111111111111111111111111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
1111111111111111111111
*///:~
```
补充两个方法：printBinaryInt()和printBinaryLong()  
```java
printBinaryInt("a: ",a);
// a,int:****, binary:****
```
#### 3.12 三元操作符if-else
boolean_f ? value0 : value1 
#### 3.13 字符串操作符+ 和 +=
注意：  
如果表达式以一个字符串起头，那么后续所有操作数都必须是字符串型
#### 3.14 使用操作符时的常犯的错误
注意： 
while(x=y){}在C或C++中可以编译通过，而在java中会指出编译错误；  
对于位运算和逻辑运算，Java可以使用按位“与”“或”代替逻辑“与”或”。
#### 类型转换操作符
注意、  
1、窄化转换（narrowing conversion）可能存在信息丢失的风险，而对于扩展转换（widening conversion），则不必显式地进行转换，因为新类型肯定能容纳原来类型的信息，不会造成信息丢失  
2、截尾和舍入  
直接输出会被截尾，使用Math.round()四舍五入 
3、表达式中出现的最大的数据类型决定了最终结果的类型
#### 3.16 Java没有sizeof
C与C++中需要使用sizeof(）的最大原因是为了“移植”，而java的所有数据类型在所有机器中的大小都是相同的
