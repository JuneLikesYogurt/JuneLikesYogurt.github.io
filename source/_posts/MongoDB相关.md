---
title: MongoDB相关
date: 2019-03-26 15:03:40
tags:
    - 数据库
categories: 
    - 数据库
---

MongoDB是一个由C++语言编写的，基于分布式文件存储的数据库系统。
分布式，即可以运行在多个服务器上存储数据副本，但只有一个是主节点，可以进行写操作。
<!-- more -->

---

MongoDB与传统的MySQL数据库载概念上有许多不同。

+ 表，table : `集合，collection`
+ 行，row : `文档，document`
+ 字段，column : `域，filed`

<br>

+ 创建数据库 `use xxx`
+ 删除数据库 `db.dropDatabase()`
+ 创建表 `db.creteCollection()`
+ 删除集合 `db.collection.drop()`
+ 更新文档 `db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})`
<br>//update()函数中，前者为当前内容，后者为将要修改为的内容。
+ 查询文档 `db.xxx.find().pretty()`
<br>//pretty()方法，格式化显示所有文档

<br>

操作符
+ $lte等进行大小的比较
    <br>`db.col.find({likes : {$gt : 100}})`
    <br>`Select * from col where likes > 100;`
+ limit()与MySQL中用途类似
+ skip()跳过xx
+ aggregate()聚合方法，$sum、$avg等计算总和、平均值。类似SQL语句中的min()、count()