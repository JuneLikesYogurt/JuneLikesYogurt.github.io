---
title: springBoot入门学习整理
date: 2020-10-09 14:39:30
tags:
    - SpringBoot
categories:
    - draft
---
## dto、dao、service、controller四层结构
### dto层
dto层/model层/entity层/pojo层，数据库实体层
dto中定义的是实体类，包含实体类的属性和对应属性的get、set方法

### dao层
数据持久层，也称为mapper层
    dao层的文件习惯以Mapper命名
dao层会调用dto层、访问数据库，dao层中会定义实际使用到的方法，比如增删改查。  
一般在dao层下还会有个叫做sqlmap的包，该包下有xml文件，文件内容正是根据之前定义的方法而写的SQL语句。

### service层
业务逻辑层，完成功能设计。
service层会对数据进行一定的处理，比如条件判断和数据筛选等等，接收dao层返回的数据，完成项目的基本功能设计。

### controller层
请求和响应控制。
controller层会调用前面三层，controller层一般会和前台的js文件进行数据的交互， controller层是前台数据的接收器，后台处理好的数据也是通过controller层传递到前台显示的。

### 参考
[Spring Boot框架model层、dao层、service层、controller层分析设计](https://blog.csdn.net/Just_learn_more/article/details/90665009?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param)
[浅析Java中dto、dao、service、controller的四层结构](https://blog.csdn.net/wyx0224/article/details/81190792)