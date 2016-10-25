###持有对象
####11.1 泛型和类型安全的容器  
对于容器的元素内容的类型判断  

####11.2 基本概念
Java容器类类库的用途是“保存对象”，并将其划分为两个不同的概念：  
1）Collection 一个独立元素的序列  
2) Map  一组承兑的“键对值”对象  

####11.3 添加一组元素
区别
> collection.addAll()  
> Collections.allAll(collection, collection,...) //第一个参数为目标集合  
 
```java
//: holding/AddingGroups.java
// Adding groups of elements to Collection objects.
import java.util.*;

public class AddingGroups {
  public static void main(String[] args) {
    Collection<Integer> collection =
      new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
    Integer[] moreInts = { 6, 7, 8, 9, 10 };
    collection.addAll(Arrays.asList(moreInts)); //快
    // Runs significantly faster, but you can't
    // construct a Collection this way://多参数添加
    Collections.addAll(collection, 11, 12, 13, 14, 15);
    Collections.addAll(collection, moreInts);
    // Produces a list "backed by" an array:
    List<Integer> list = Arrays.asList(16, 17, 18, 19, 20);
    list.set(1, 99); // OK -- modify an element
    // list.add(21); // Runtime error because the
                     // underlying array cannot be resized.
  }
} ///:~
```

Arrays.asList()函数：
```java
//: holding/AsListInference.java
// Arrays.asList() makes its best guess about type.
import java.util.*;

class Snow {}
class Powder extends Snow {}
class Light extends Powder {}
class Heavy extends Powder {}
class Crusty extends Snow {}
class Slush extends Snow {}

public class AsListInference {
  public static void main(String[] args) {
    List<Snow> snow1 = Arrays.asList(
      new Crusty(), new Slush(), new Powder());

    // Won't compile:
    // List<Snow> snow2 = Arrays.asList(
    //   new Light(), new Heavy());
    // Compiler says:
    // found   : java.util.List<Powder>
    // required: java.util.List<Snow>

    // Collections.addAll() doesn't get confused:因为第一个参数已经知晓了它的类型snow
    List<Snow> snow3 = new ArrayList<Snow>();
    Collections.addAll(snow3, new Light(), new Heavy());

    // Give a hint using an
    // explicit type argument specification:
    List<Snow> snow4 = Arrays.<Snow>asList(
       new Light(), new Heavy());
  }
} ///:~
```

####11.4 容器的打印
直接打印  
####11.5 List
两种：  
* Arraylist: 随机访问快，插入和移除元素时慢,代价很大  
* LinkedList: 插入和移除快，随机访问比较慢
```java
//: holding/ListFeatures.java
import typeinfo.pets.*;
import java.util.*;
import static net.mindview.util.Print.*;

public class ListFeatures {
  public static void main(String[] args) {
    Random rand = new Random(47);
    List<Pet> pets = Pets.arrayList(7);//自定义的方法，忽略
    print("1: " + pets);
    Hamster h = new Hamster();
    pets.add(h); // Automatically resizes
    print("2: " + pets);
    print("3: " + pets.contains(h));
    pets.remove(h); // Remove by object
    Pet p = pets.get(2);
    print("4: " +  p + " " + pets.indexOf(p));
    Pet cymric = new Cymric();
    print("5: " + pets.indexOf(cymric));
    print("6: " + pets.remove(cymric)); //remove()方法返回值为true或false
    // Must be the exact object:
    print("7: " + pets.remove(p));
    print("8: " + pets);
    pets.add(3, new Mouse()); // Insert at an index 指定位置插入
    print("9: " + pets);
    List<Pet> sub = pets.subList(1, 4); //得到一个子列表
    print("subList: " + sub);
    print("10: " + pets.containsAll(sub)); //判断是否包含全部内容
    Collections.sort(sub); // In-place sort 排列
    print("sorted subList: " + sub);
    // Order is not important in containsAll():
    print("11: " + pets.containsAll(sub));
    Collections.shuffle(sub, rand); // Mix it up打乱
    print("shuffled subList: " + sub);
    print("12: " + pets.containsAll(sub));
    List<Pet> copy = new ArrayList<Pet>(pets);
    sub = Arrays.asList(pets.get(1), pets.get(4));
    print("sub: " + sub);
    copy.retainAll(sub); //交集
    print("13: " + copy);
    copy = new ArrayList<Pet>(pets); // Get a fresh copy
    copy.remove(2); // Remove by index
    print("14: " + copy);
    copy.removeAll(sub); // Only removes exact objects
    print("15: " + copy);
    copy.set(1, new Mouse()); // Replace an element代替
    print("16: " + copy);
    copy.addAll(2, sub); // Insert a list in the middle加入列表
    print("17: " + copy);
    print("18: " + pets.isEmpty());
    pets.clear(); // Remove all elements
    print("19: " + pets);
    print("20: " + pets.isEmpty());
    pets.addAll(Pets.arrayList(4));
    print("21: " + pets);
    Object[] o = pets.toArray();
    print("22: " + o[3]);
    Pet[] pa = pets.toArray(new Pet[0]);
    print("23: " + pa[3].id());
  }
}
```
####11.6 迭代器
Iterator能够将遍历序列的操作与序列底层的结构分离  
hasNext()用于判断聚合对象中是否还存在下一个元素，为了不抛出异常，在每次调用next()之前需先调用hasNext()，如果有可供访问的元素，则返回true；   
next()方法用于将游标移至下一个元素，通过它可以逐个访问聚合中的元素，它返回游标所越过的那个元素的引用；   
remove()方法用于删除上次调用next()时所返回的元素。
#####11.6.1 ListIterator
双向、可规定起始位置  
```java
//: holding/ListIteration.java
import typeinfo.pets.*;
import java.util.*;

public class ListIteration {
  public static void main(String[] args) {
    List<Pet> pets = Pets.arrayList(8);
    ListIterator<Pet> it = pets.listIterator();
    while(it.hasNext())
      System.out.print(it.next() + ", " + it.nextIndex() +
        ", " + it.previousIndex() + "; ");
    System.out.println();
    // Backwards:反向
    while(it.hasPrevious())
      System.out.print(it.previous().id() + " ");
    System.out.println();
    System.out.println(pets);	
    it = pets.listIterator(3);//规定起始位置
    while(it.hasNext()) {
      it.next();
      it.set(Pets.randomPet());
    }
    System.out.println(pets);
  }
} /* Output:
Rat, 1, 0; Manx, 2, 1; Cymric, 3, 2; Mutt, 4, 3; Pug, 5, 4; Cymric, 6, 5; Pug, 7, 6; Manx, 8, 7;
7 6 5 4 3 2 1 0
[Rat, Manx, Cymric, Mutt, Pug, Cymric, Pug, Manx]
[Rat, Manx, Cymric, Cymric, Rat, EgyptianMau, Hamster, EgyptianMau]
*///:~
```
####11.7 LikedList
####11.8 Stack
栈（后进先出），起始LinkedList就能够实现栈的所有功能的方法(java.util.stack)   
但是这样的stack会产生LinkedList的其他所有方法的类（Java1.0即使用了此版本）  
```java
//: net/mindview/util/Stack.java
// Making a stack from a LinkedList.
package net.mindview.util;
import java.util.LinkedList;

public class Stack<T> {
  private LinkedList<T> storage = new LinkedList<T>();
  public void push(T v) { storage.addFirst(v); }
  public T peek() { return storage.getFirst(); }//不移除
  public T pop() { return storage.removeFirst(); }
  public boolean empty() { return storage.isEmpty(); }
  public String toString() { return storage.toString(); }
} ///:~
```
我们使用这个Stack类，它存在于 net.mindview.util，建立对象时要写包名。  
或者通过显示的导入来控制首选，import net.mindview.util.stack    
不过现在任何对Stack的引用都将选择使用 net.mindview.util 版本  
```java
//: holding/StackTest.java
import net.mindview.util.*;

public class StackTest {
  public static void main(String[] args) {
    Stack<String> stack = new Stack<String>();
    for(String s : "My dog has fleas".split(" "))
      stack.push(s);
    while(!stack.empty())
      System.out.print(stack.pop() + " ");
  }
} /* Output:
fleas has dog My
*///:~
```
####11.9 Set
* Set不存在重复的元素
* 查找是Set最重要的操作，而且我们通常选择一个HashSet实现
* Set 具有和Collection一模一样的接口，不象List，没有额外的功能
* TreeSet将元素存储在红-黑树数据结构中，而HashSet使用的是散列函数。
* LinkedHashList因为查询速度的原因也使用了散列，但是看起来它使用了链表来维护元素的插入顺序  
```java
//: holding/UniqueWordsAlphabetic.java
// Producing an alphabetic listing.
import java.util.*;
import net.mindview.util.*;

public class UniqueWordsAlphabetic {
  public static void main(String[] args) {
    Set<String> words =
      new TreeSet<String>(String.CASE_INSENSITIVE_ORDER);//字母序排列
    words.addAll(
      new TextFile("SetOperations.java", "\\W+"));//正则表达式，表示一个或多个字母
    System.out.println(words);
  }
} /* Output:
[A, add, addAll, added, args, B, C, class, Collections, contains, containsAll, D, E, F, false, from, G, H, HashSet, holding, I, import, in, J, java, K, L, M, main, mindview, N, net, new, Output, Print, public, remove, removeAll, removed, Set, set1, set2, SetOperations, split, static, String, to, true, util, void, X, Y, Z]
*///:~
```

####11.10 Map
键、值

####11.11 Queue
* 队列（先进先出）,通过LinkedList向上转型为Queue
* offer(c) 插入队尾
* peek()和element() 不移除的情况下返回队头
* poll()和remove() 移除并返回队头
#####11.11.1 PriorityQueue
* offer()时，会被排序，自然排序
```java
//: holding/PriorityQueueDemo.java
import java.util.*;

public class PriorityQueueDemo {
  public static void main(String[] args) {
    PriorityQueue<Integer> priorityQueue =
      new PriorityQueue<Integer>();
    Random rand = new Random(47);
    for(int i = 0; i < 10; i++)
      priorityQueue.offer(rand.nextInt(i + 10));
    QueueDemo.printQ(priorityQueue);

    List<Integer> ints = Arrays.asList(25, 22, 20,
      18, 14, 9, 3, 1, 1, 2, 3, 9, 14, 18, 21, 23, 25);
    priorityQueue = new PriorityQueue<Integer>(ints);
    QueueDemo.printQ(priorityQueue);
    priorityQueue = new PriorityQueue<Integer>(
        ints.size(), Collections.reverseOrder());
    priorityQueue.addAll(ints);
    QueueDemo.printQ(priorityQueue);

    String fact = "EDUCATION SHOULD ESCHEW OBFUSCATION";
    List<String> strings = Arrays.asList(fact.split(""));
    PriorityQueue<String> stringPQ =
      new PriorityQueue<String>(strings);
    QueueDemo.printQ(stringPQ);
    stringPQ = new PriorityQueue<String>(
      strings.size(), Collections.reverseOrder());//反向排序的Comparator
    stringPQ.addAll(strings);
    QueueDemo.printQ(stringPQ);

    Set<Character> charSet = new HashSet<Character>();
    for(char c : fact.toCharArray())
      charSet.add(c); // Autoboxing
    PriorityQueue<Character> characterPQ =
      new PriorityQueue<Character>(charSet);
    QueueDemo.printQ(characterPQ);
  }
} /* Output:
0 1 1 1 1 1 3 5 8 14 //默认为自然排序
1 1 2 3 3 9 9 14 14 18 18 20 21 22 23 25 25
25 25 23 22 21 20 18 18 14 14 9 9 3 3 2 1 1
       A A B C C C D D E E E F H H I I L N N O O O O S S S T T U U U W
W U U U T T S S S O O O O N N L I I H H F E E E D D C C C B A A
  A B C D E F H I L N O S T U W
*///:~
```

####11.12 Collection和Iterator
优越！

####11.13 Foreach与迭代器
* 可以用于数组，Collection对象，原因是Java SE5中引入的新的被称为Iterable的接口，  
该接口能够产生Iterabel的iterator()方法，并且Iterable接口被foreach用来在序列中移动。  
* 但是数组不是一个Iterable

####11.14 总结
* List :大量访问用ArrayList, 频繁插入用LinkedList
* 各种Queue以及栈的行为，由LinkedList提供支持
* Map : 快速访问用HashMap;而TreeMap的键始终保持排序状态所以没有HashMap快。
LinkedHashMap保持元素插入的书序，但是也通过散列提供快速访问能力
* Set ：不接受重复元素，HashSet提供最快的查询速度，而TreeSet保持元素处于排序状态，LinkedHashSet以插入顺序保存元素

