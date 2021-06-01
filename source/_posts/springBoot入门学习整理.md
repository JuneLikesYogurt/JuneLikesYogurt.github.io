---
title: springBoot入门学习整理
date: 2020-10-09 14:39:30
tags:
    - SpringBoot
categories:
    - Notes
---
# dto、dao、service、controller四层结构
## dto层
dto层/model层/entity层/pojo层，数据库实体层
dto中定义的是实体类，包含实体类的属性和对应属性的get、set方法  
DTO并不对应一个实体，可能仅存储实体的部分属性或加入符合传输需求的其他的属性。
数据传输对象DTO本身并不是业务对象。数据传输对象的`根据UI的需求`进行设计的

## dao层
数据持久层，也称为mapper层
    dao层的文件习惯以Mapper命名
dao层会调用dto层、访问数据库，dao层中会定义实际使用到的方法，比如增删改查。  
一般在dao层下还会有个叫做sqlmap的包，该包下有xml文件，文件内容正是根据之前定义的方法而写的SQL语句。

## service层
业务逻辑层，完成功能设计。
service层会对数据进行一定的处理，比如条件判断和数据筛选等等，接收dao层返回的数据，完成项目的基本功能设计。

## controller层
请求和响应控制。
controller层会调用前面三层，controller层一般会和前台的js文件进行数据的交互， controller层是前台数据的接收器，后台处理好的数据也是通过controller层传递到前台显示的。


    【domain】目录主要用于实体（Entity）与数据访问层（Repository）控制

## 参考
[Spring Boot框架model层、dao层、service层、controller层分析设计](https://blog.csdn.net/Just_learn_more/article/details/90665009?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param)
[浅析Java中dto、dao、service、controller的四层结构](https://blog.csdn.net/wyx0224/article/details/81190792)

---

### Map
    Interface Map<K,V> 
    K - key的类型 
    V - value的类型
+ get(Object key)
  + 返回指定键映射到的值，或者null此映射是否不包含键的映射
+ hashCode()
+ isEmpty()
+ keySet()
  + 返回Set此映射中包含的键的视图
+ put(K key, V value)
  + 将指定的值value与此映射中的指定键key相关联（可选操作）


### redis
    Redis 是一个高性能的key-value数据库
Redis支持主从同步。数据可以从主服务器向任意数量的从服务器上同步，从服务器可以是关联其他从服务器的主服务器。

### ESB
    （Enterprise Service Bus）全名：企业服务总线。是SOA架构思想的一种实现思想，面相企业应用集成。
    一般采用集中式转发请求，适合大量异构系统集成，侧重任务的编排，性能问题可通过异构的方式来进行规避，无法支持特别大的并发


### validated
https://zhuanlan.zhihu.com/p/37303652