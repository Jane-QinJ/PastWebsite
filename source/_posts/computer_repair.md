---
title: Computer repair
date: 
tags:
- software repair

categories:
- daily repair
---

此篇文章用于记录日常遇到的软件安装问题，包括各种路径冲突、盘符扩充等等

This article used to record the problem that I faced in my daily development life.

1. How to enlarge the C partion space.
  如何扩充C盘空间？

重装系统后， 所有的软件都需要重新下载， 但是C盘分区时分的过小，导致许多软件的系统配置文件无法安装到C盘。

搜索一番后，找到一款很棒的分区软件[MiniTool Partition Wizard](http://www.partitionwizard.com/ )

具体操作可参考[链接](https://blog.csdn.net/a1b2c3d4123456/article/details/49763883)

2. MySQL reload.s
[Navicat连接mysql8.0.1版本出现1251--Client does not support authentication protocol requested by server的解决](https://blog.csdn.net/qq_42152399/article/details/80360817)

3. MYSQL **The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone**

**Solution**
时区问题,只需更改时区即可
[Reference](http://www.cnblogs.com/wangdianqian/p/9927406.html)

```
mysql -uroot -p
pw:123123

set global time_zone = ‘+8:00‘; ##修改mysql全局时区为北京时间，即我们所在的东8区
set time_zone = ‘+8:00‘; ##修改当前会话时区
flush privileges; #立即生效
```

4. 重装Mysql
[重装mysql](https://www.cnblogs.com/jpfss/p/6652701.html)