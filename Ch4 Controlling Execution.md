第四章 控制执行流程
#### 4.1 true和false
#### 4.2 if-else
#### 4.3 迭代
#### 4.4 Foreach语法
```java
for(int value: table){}
```
任何返回一个数组的方法都可以使用foreach，例如String类中有toCharArray()，它返回一个char数组  
```java
for(char c : "An African Swallow".toCharArray() )
      System.out.print(c + " ");
```
#### 4.5 return
#### 4.6 break和continue
break用于强行退出循环，continue则停止执行当前的迭代
#### 4.7 臭名昭著的goto
