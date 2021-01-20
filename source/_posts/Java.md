---
title: 关于Java类中数组对象创建的疑问
date: 2019-01-11 21:04:05
tags:
- java

categories: Java
---


---
```java
public class Mix4{
  int counter = 0;
  public static void main(String[] args){
    int count = 0;
    Mix4 [] m4a = new Mix4[20];
    int x = 0;
    while(x<9){
      m4a[x] = new Mix4();
      m4a[x].counter = m4a[x].counter + 1;
      count = count + 1;
      count = count + m4a[x].maybeNew(x);
      x = x + 1;
    }
  System.out.println(count+" "+m4a[1].counter);
  }
  
  public int maybeNew(int index){
      if(index<5){
        Mix4 m4 = new Mix4();
        m4.counter = m4.counter + 1;
        return 1;
      }
      return 0;
    }

}
```
这是Head First Java 中的一个习题， 在解答的过程中发现自己不了解以下几个知识点：

- 类实例变量的初始化
- 数组对象的创建

对于第一个问题，**类实例变量的初始化**，在可视化的过程中我有了新的理解，先放上整个过程的结果。
![代码执行的图示](https://raw.githubusercontent.com/Jane-QinJ/Jane-QinJ.github.io/master/images/1.png)

下面写下我在执行这个程序中遇到并解决的一些问题
1. **程序是从main()开始的**
	（这点我之前一直默认自己知道，但是程序真正执行起来，才发觉自己对这点的疏忽，还是需要谦虚些，默认自己什么都不知道，每个点都要踏踏实实扣下去）
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190113151016479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1FpbnFpblRheWxvcg==,size_16,color_FFFFFF,t_100)

2. **数组是一个对象**， 无论被声明来承载的是primitive主类型数据或对象引用，数组永远是对象。
数组对象可以有primitive主数据类型的元素，但数组本身绝对不会是primitive主数据类型， 不管数组带有什么， 它一定是对象。



这句话的理解，在我看来可以这样解释：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190113151905260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1FpbnFpblRheWxvcg==,size_16,color_FFFFFF,t_70)



```
Mix4 [] m4a = new Mix4[20];
```

这段程序可以分为三部分
1. 对象的声明
2. 对象的创建
3. 赋值

- 对象的声明
```
Mix4[] m4a
```

声明了一个**类型**为Mix4[]的**数组变量**， 它的名称是m4a.
> 这里补充一下，java注重类型， 声明变量必须有类型和名称（variables must have a type, variables must have a name）
[to be continued....]()

- 对象的创建

```
new Mix4[20]
```
创建大小为20的数组(这有着20个存储空间整体，为一个Mix4数组对象）
而这20个存储空间可以用来存储类型为Mix4的对象

这里只保存着Mix4的引用
相当于只声明了对象,没有创建实际的Mix4对象。
>可以这么理解
```
Mix4 m4a[0];
```
>数组中第一个索引里存放的是Mix4的引用  
>要创建Mix4对象的话 还要
```
m4a[0] = new Mix4();
```

- 赋值
连接对象和引用， 将Mix4[20]这个大小为20的数组，赋值给之前声明为Mix4[]的数组变量m4a.

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190113154401235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1FpbnFpblRheWxvcg==,size_16,color_FFFFFF,t_70)
从上图中， 我们看到m4a这个数组变量，相当于一个遥控器，它指向了数组对象（也就是Mix4[20]），这个数组变量也称引用变量。

我们可以把变量看做杯子，变量用来装东西，也就是往杯子中装东西。
比如： 
```
bype x = 7;
```
就是代表数值7的字节（00000111）被放进变量中。

只不过这个引用变量装的东西比较特别，它放的东西代表**取得Mix4[20]对象的方法**以字节形式放进变量中，也就相当于一个遥控器， 这个引用变量可以通过 圆点“.”运算符来引用对象的方法和实例变量。

在数组中，对数组的操作可以不需要变量名称。只需要数组索引（位置）就可以操作特定对象：
```
Mix4 [] m4a = new Mix4[20];
m4a[0] = new Mix4();
m4a[0].counter = m4a[0].counter + 1;
```
这段代码代表了程序第一次执行while循环时，x=0情况下，Mix4数组变量m4a[0]对Mix4对象的操作。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190113160332134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1FpbnFpblRheWxvcg==,size_16,color_FFFFFF,t_70)
注意：
Mix4[20]数组中存放的仅仅是对Mix对象的引用， 还需要创建实际的Mix对象
```
new Mix4()
```
，并把它赋值给数组的元素
```
m4a[0] = new Mix4()
```

我理解错误的点主要在于对类实例变量的理解。
我把实例变量counter看做每个对象都可以进行修改的值，错误的把counter进行了累加。

实际上，每个类是对象的蓝图。 创建一个对象就是对类的实例化。 这个对象会有自己的实例变量和方法。

对象本身一致的事物被称为：实例变量（instance variable）
对象可以执行的动作称为：方法（methods）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190113161209191.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1FpbnFpblRheWxvcg==,size_16,color_FFFFFF,t_70)
可以看到程序进行到第8行时，new Mix4()创建了一个Mix4类型的对象， 它执行了两次初始化\<init:2>, 并且这个对象根据Mix4这个类的蓝图， 初始化了自己的实例变量counter = 0.

之后将这个初始化了的对象赋值给了引用变量m4a[0]
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190113161432105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1FpbnFpblRheWxvcg==,size_16,color_FFFFFF,t_70)

所以每一个数组引用变量都会有一个Mix4对象， 这些对象虽然是同一个类， 但每个都有自己独立的实例变量， 也就是counter. 而这个counter在创建对象时都会被赋值为0.

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190113162729536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1FpbnFpblRheWxvcg==,size_16,color_FFFFFF,t_70)

```
Mix4 [] m4a = new Mix4[20];
m4a[x] = new Mix4();
m4a[x].counter = m4a[x].counter + 1;
```
现在重看这行代码， 你是否能有新的理解呢

---

如有错误地方，请大家指正。
共同学习，共同进步，期待大家的issue~