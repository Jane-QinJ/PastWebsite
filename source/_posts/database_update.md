---
title: Database basic operation
date:
tags: 
- database

categories: Database
---

1. MySQL 的简介与安装

123123

登录：mysql -uroot -p
之后输入密码
Ctrl+C 强制退出

2. 数据库和表的操作

如何创建数据库

```
create database test;
```

这就代表创建了一个名为 test 的数据库

```
show databases;
```
查看当前所有的数据库

```
drop database test;;
```
删除数据库

表操作
use test;
在操作表之前，首先我们要连接一个数据库
出现 Database changed 字样表示成功，当我们忘记当前处于哪个数据库的时候，可以通过 select 来查看当前数据库

如何建表
create table if not exists 表名(

);

这里是创建了一个成绩表，名为 exam_score，并设置了 id 为自增主键
show columns from exam_score 
这个命令可以展示表的结构
跟数据库的操作一样，show tables 可以显示当前存在的表有哪些

删除表
drop table exam_score;
3. 插入和查找数据

向表中插入数据格式：
insert into 表名 values 数据

查看表中数据 ：
select * from exam_score;

查找：
获取班级内学号前两位的同学：

第一种方法用到了 order by 子句
通过字面意思也看的出来，根据...排序
我们可以将任意字段当作排序的条件，它默认是按照升序排列的
降序排列
select * from 表名 order by 字段名 desc;
升序的话，后面接 asc

获取班级里叫做‘Li’的同学
要用到 where 子句


提示：再出现select语句无法结束的情况，可以输入'\c，因为在之前的语句中，你可能没有闭合引号，所以MySQL把它作为正在收集的字符串的一部分，就无法结束。

查询有条件的数据
如果班级中又来了一位名字叫‘Zhao’的同学，不过他的成绩不好，我要查询这个同学的数据该怎么办
首先我们插入一个数据 
insert into exam_score values (5,'Zhao',45.50);
然后 where 子句又派上用场了

可以类比成条件判断中的 if 语句  where表示一种条件约束
4. 修改和删除数据

修改数据：
update 表名 set 要修改的数据项 where 条件

也可以同时更改多个字段的数据，赋值语句中间用逗号(,)隔开就好了
还可以用 replace 来替换


删除数据
delete from 表名 where 选取条件

把表中数据全删除
delete from 表名;


清空表数据和删除表的区别：
delete 清空操作只是数据被删除了，表还存在
 drop 操作是删除了整个表，当表中存在数据时，要慎重使用这个方法
5. 修改表结构

增加表字段
我们的考试分数表中要添加一个‘其他分数’的字段，而且默认值是 10，该怎么操作呢
这就要用到 alter

命令格式：
alter table 表名 add 字段名 数据类型 default 默认值;
修改表字段
校决定把这个其他分数改为评价，默认值不是数字了，变成了 normal，这该怎么办呢

命名格式：
alter table 表名 change 旧字段 新字段 新数据类型 default 默认值;

删除表中字段

命名格式：
alter table 表名 drop column 字段名;

修改表名

命名规范：
rename table 旧表名 to 新表名;
6. 表的连接
实际开发中，一张表往往是不足以解决我们的问题的，可能需要两张或多种表
我们来看看，如何将两张表关联起来
创建两个表，并插入数据
score表

info表




表的连接：
inner join
等值连接、内连接

select score.id,score.name,info.phone from score inner join info on score.name = info.name;
select 表A.字段1,表A.字段2,表B.字段3 from 表A inner join 表B on 表A.字段2 = 表B.字段2;
这段命令就表示，我通过 inner join 连接了 score 和 info 两个表
读取了表 score 中的字段 id 和 name ，和在表 info 中对应的 phone 字段值
left join
左连接
读取left join 左边数据表所有选取的字段，即使右边数据表中没有对应的字段值
right join
右连接
读取right join 右边数据表中所有选取的字段，即使左边数据表中没有对应的字段值

给 info 表添加两条数据，让你更容易看出左连接、右连接、等值连接的区别



7. 索引
索引是一种单独的、物理的对数据库表中一列或多列的值进行排序的一种存储结构
本书会有目录，我们可以根据目录找到对应的章节
索引就好比数据表的目录

添加索引

格式
create index 索引名 on 表名(字段名);

查看索引
格式
show index from score; 
也可以在创建表的时候直接指定索引


删除索引

格式：
drop index 索引名 on 表名

通过 alter 命令创建、删除索引

alter table 表名 add 索引名(字段名)
alter table 表名 drop index 索引名
刚才我们创建的都是普通索引，还有一种是唯一索引
唯一索引
create unique index 索引名 on 表名(字段名);

和普通索引的区别
顾名思义，二者的区别就是唯一索引的值必须唯一，但可以有空值
我们练习操作的数据库中数据都比较少，可能无法看出速度上的差异
但是像淘宝、京东等大型网站的数据库，其中数据可能是几亿条，通过索引便可以快速的找到所需数据
8. 外键

外键：
如果一个表的某个字段指向另一个表的主键，就称之为外键
被指向的表，称之为主表，也叫父表
那么另一个表就是从表，也叫子表

如何设置和使用一个外键
我们通过这样一个例子来演示一下，两个表
一个是作者表 author_table
一个是文章表 article_table

首先创建作者表，有作者的 id 和 name，如下图	

然后创建文章表，注意了，看看我是如何设置了一个外键

这里有文章的 id 和 title，还有作者的 id
黄色框出的部分表示将表中的 author_id 字段指向 author_table 的主键，即设置了一个外键
然后我们来添加一些数据

然后我们看一下表中的数据

那下面我们在向文章表中插入一个作者 id 为 5 的数据，看看能否成功添加

失败了
错误信息告诉我们，主表中没有 id 为 5 的作者，添加一个作者 id 是 5 的文章数据自然是不可以的
也因为这个外键的约束，我们要想删除父表中 id 为 4 的数据也是不可以的
因为子表中有对应作者 id 为 4 的文章数据

如何查看、删除一个表的外键

命令格式：
show create table 表名; 
通过这个命令我们可以看到表中存在的外键

删除一个表的外键

上图的命名删除了表中的外键约束
命令格式
alter table 表名 drop foreign key 外键名;
删除外键之后，又可以自由的添加、删除数据了
因为外键删除后不会对表中的数据有任何影响，这只是改变了对表的约束

外键的作用还很多，我们可以通过外键给两表设置级联操作

开始我们是在建表时就指定了外键
现在是在刚刚的删除操作后，用 alter 命令添加了一个外键

on delete cascade 
on update cascade
cascade
 表示关联操作，即如果父表中数据被删除或更新，子表中对应数据也会执行同样的操作
像这样的关键词还有两个
set null
表示子表数据不指向父表任何记录 
restrict
表示拒绝主表的相关操作
当不加这两句话时，默认就是 restrict，这也就是为什么开始我们主表中数据无法删除的原因

设置完级联操作后，删除作者表中一条数据，看看结果时什么样的

删除了父表中的一条数据，子表中外键与之对应的数据也被删除了