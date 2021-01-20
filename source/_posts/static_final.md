---
title: Java中静态(static)变量、方法与实例变量、方法的规范
tags:
- java

categories: Java
---


Head First java第十章的静态变量与方法一节中， 规定了许多规则， 通过这几道有疑问的习题来加深印象。

已增加书籍连接（中英版）， 在文末附上链接， 大家按需提取， 请勿商用。

1. 能否在非静态方法中存取静态变量？
```java
//在非静态方法中调用静态变量
//结果：通过编译
public class Foo{
    static int x;
    public void go(){
	System.out.println(x + "success");
}
public static void main(String[] args){
//	Foo.go(); 静态方法不能引用非静态方法
}
}
```
![](https://img-blog.csdnimg.cn/20190129220155138.png)
**结果： 通过编译**
**结论1： 可以在非静态方法中读取静态变量**

> A non-static method in a class can always call a static method in the class or access a static variable of the class.

译：一个类中的**非静态方法**能够调用这个类的**静态方法**或存取这个类的**静态变量**

吐槽一下， 这本书是很好的， 但是中文译版有些错误或语序不通。。。比如这里此书作者的翻译前后居然相互矛盾。
之后会继续补充链接， 给大家一个英文扫描版， 推荐有能力的同学选择中英对照。

2. 能否在静态方法中调用实例变量
```java
//静态方法中调用实例变量
public class Foo2{
    int x;
    public static void go(){
        System.out.println(x);
}
    public static void main(String[] args){

}
}
```
![](https://img-blog.csdnimg.cn/20190129221508213.png)
**结果： 出现编译错误**
**结论2： 无法从静态方法中调用非静态变量**

3.  非静态方法能否存取(access)final变量
```java
// 非静态方法能否存取(access)final变量
public class Foo3{
    final int x;
    public void go(){
        System.out.print(x);
}
public static void main(String[] args){}
}
```
![](https://img-blog.csdnimg.cn/20190129222705355.png)
**结果: 编译错误， final变量必须初始化**
- **尝试初始化**
1. 声明时初始化
```java
// 非静态方法能否存取(access)final变量
public class Foo3{
    final int x = 0;  //初始化为0
    public void go(){
        System.out.print(x);
}
public static void main(String[] args){}
}
```
![](https://img-blog.csdnimg.cn/20190129223031163.png)
**结果:** 编译通过

2. 在静态初始化程序中 初始化
```java
// 非静态方法能否存取(access)final变量
public class Foo3{
 
    final int x ;
       static { //静态初始化程序
        x = 0;
}
    public void go(){
        System.out.print(x);
}
public static void main(String[] args){}

}
```
![](https://img-blog.csdnimg.cn/20190129223517288.png)
出错， static没有把final变量初始化了，又尝试修改， 这次增加了static关键字在final变量中。
try again:
```java
// 非静态方法能否存取(access)final变量
public class Foo3{
 
   static final int x ;  //变为静态final变量
       static {
        x = 0;
}
    public void go(){
        System.out.print(x);
}
public static void main(String[] args){}

}
```
![](https://img-blog.csdnimg.cn/2019012922395150.png)
**结果： 编译成功**

**结论3: 非静态方法能够存取final变量， 但是final变量必须被初始化。 final变量初始化方式有两种：
第一种： 声明时初始化
第二种： 将其声明为静态final变量， 使用static{}静态初始化程序进行初始化（她会在其他程序可以使用该类之前就执行）**

4. 非静态方法是否可以存取静态final变量
```java
public class Foo4{
    static final int x = 12;
    public void go(){
        System.out.print(x);
}
}
```
![](https://img-blog.csdnimg.cn/20190129224836892.png)
**结果： 编译通过**

试着破坏（改变）这个代码：
比如， 
- 把它的变量初始化值去掉：
```java
public class Foo4{
    static final int x ;
    public void go(){
        System.out.print(x);
}
}
```
![](https://img-blog.csdnimg.cn/20190129225013264.png)
果然出错了， 从上一个例子我们知道是因为**final变量必须初始化**
- 再试着把static去掉
```java
public class Foo4{
    final int x = 12;
    public void go(){
        System.out.print(x);
}
}
```
![](https://img-blog.csdnimg.cn/20190129225228266.png)
编译成功， 这说明在**非静态方法中可以存取final变量**
**结论4：非静态方法中可以存取静态 [static]final变量和final变量**

5. 非静态方法参数中的final变量能否被该方法存取
```java
public class Foo5{
    static final int x = 12; //静态final变量， 相当于常数
    public void go(final int x){  //方法形式参数， final变量
        System.out.print(x);
}

public static void main(String[] args){
    Foo5 foo = new Foo5();
    foo.go(5); //传入5
}
}
```
这个类中， 非静态方法go的参数为final的int型变量， 它被输出语句输出。
注意这里输出的 x为 调用go方法的对象foo传入的实际参数5， 而不是实例变量中的12.
![](https://img-blog.csdnimg.cn/201901292304488.png)
**结论5： 非静态方法参数中的final变量能被该方法存取**

6. 静态 方法参数中的final变量能否被该方法存取
```java
public class Foo6{
    int x = 12;
    public static void go(final int x){ 
        System.out.println(x);  
}
public static void main(String[] args){ //静态方法可以调用静态方法
    Foo6 foo = new Foo6();
    foo.go(5);
}
}
```

![](https://img-blog.csdnimg.cn/20190129231008281.png)
结果： 编译成功
**结论6：静态方法参数中的final变量能被该方法存取**

- **life和scope的差别**
在方法中， 
作用域（scope）范围内的变量不受方法外部的实例变量影响（即使同名）。 这个被称为局部变量， 可以把参数也看作是一个局部变量， 它们跟随着方法 从被调用 到 执行完而被丢弃（调用栈弹出）

局部变量的范围（scope）只限于声明它的方法之内。 当此方法调用别的方法时， 该变量还活着（In **life's** perspective is **alive**），但不在目前的范围内（scope）。执行其他方法完毕返回时， 范围也就跟着回来。

方法还有一个life(生命周期)， 只要变量的堆栈块还在堆栈上， 局部变量就算活着， 它会活到方法执行完毕为止。

当局部变量还活着（life层面)的时候， 它的状态会被保存。

只要go()还在堆栈上， x变量就会保存它的值， 但x变量只能在go()方法待在栈顶时才能使用。 也就是说， 局部变量只能在声明它的方法 在执行中 才能被使用。

[链接 中文版：](https://pan.baidu.com/s/1cHSUNy0OgffOrCd38UaDSw) 
提取码：pi8j 

[链接 英文版](https://pan.baidu.com/s/1-tSXcvp8Z_qEk7pUYo0ZOg)
提取码：olvs 
