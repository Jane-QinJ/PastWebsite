---
title: Basic C note
date: 2019-7-5
tags:
- learning note

categories: learning note
---

## 一、 文本显示

- printf用于输出文字
```
#include <stdio.h>
int main()
{
    printf("我是小仙女,\n我长得很漂亮。\n");
    return 0;
}
```
## 二、数据类型

```
//standard io标准流输出
#include <studio.h>
int main()
{
	//char 字符类型，主要用来显示给人看
   //int 整型， 整数型数字， 可以用来计算。
   printf("hello\n");
   printf("%d\n",1);
   printf("%d大于%d",2,1); //2替换第一个%d,1替换第二个%d
}
```
- 要显示字符型，我们只要直接写在两个引号里就可以了，写了什么就显示什么。
- %d是占位符，显示的时候，逗号后面的内容会替换%d。

## 三、变量存储

```
#include <stdio.h>
int main()
{
    int weight; //定义一个变量weight
    weight = 170; //给weight赋值为170
    printf("%d\n",weight); //显示weight的值。
    weight = weight+10;
    printf("%d\n",weight);
}
```

![1_memory](https://raw.githubusercontent.com/Jane-QinJ/Jane-QinJ.github.io/master/images/basicC_images/1_memory.png)

