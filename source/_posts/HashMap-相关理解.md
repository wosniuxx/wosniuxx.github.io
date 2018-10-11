---
title: HashMap 相关理解
date: 2018-10-11 11:20:33
tags: [JAVA]
---
# HashMap, ConcurrentHashMap, HashTable
#JVM
# hashing概念
哈希散列、哈希算法，根据某个对象生成一串数据

# HashMap的工作原理
HashMap是基于hashing原理，使用put(),get()存储和读取对象，当使用put()传递key和value时，首先对key调用hashCode()方法，根据返回的hashCode找到bucket中存储Entry对象。
注：buctet中存放的是entry对象；当出现hashcode碰撞时，存放该键值对的链表。

# HashMap解决碰撞（hashCode碰撞解决办法）
当出现hashcode碰撞时，在bucket存放该键值对（entry）的链表。

hashCode碰撞解决办法：
开放地址法、拉链法、再次计算哈希值

<!-- more -->

# equals()及hashCode()的应用，已经在HashMap中的重要性
hashCode()即比较hashCode的值， equals()比较的是hashCode及对象值；
hashMap使用hashCode寻找bucket，如遇到hashcode碰撞，在链表中使用equals()寻找正确的对象。

# 不可变对象的好处
减少比较equals()，提高hashMap的效率。
为了要计算hashCode()，就要防止键值改变，如果键值在放入时和获取时返回不同的hashcode的话，那么就不能从HashMap中找到你想要的对象。不可变性还有其他的优点如线程安全。

# HashMap重新调整
负载因子默认为0.75（75%），HashMap的大小超过了负载因子(load factor)定义的容量，会进行一次rehashing,创建一个原先hashMap两倍大小的map，并将原先对象放入新的bucket中。

# HashMap多线程的条件竞争
两个线程同时对同一个hashMap进行rehashing时，在调整大小的过程中，储存在链表中的元素次序会反过来，因此移动到新的bucket位置，HashMap并非将元素放在链表的尾部，而是放在头部，这是为了避免尾部遍历。如果条件竞争发生了，就会陷入死循环。
在jdk8中，已经很好的解决了避免解决尾部遍历的问题，在链表中加入指针，避免每次循环查询链表尾部。

# HashMap的底层原理
HashMap底层是数组加引用，和链表组成的。
当计算hashcode时，通过对象的hashcode值，并且和数组的长度-1做一次与元算。得到的值减少碰撞几率。
初始容量为16，负载因子默认0.75
容量建议2的n次幂，减少空间浪费，可以使得元素分布更均匀。
当容量*负载因子<元素，发生resize
写的真牛逼
[深入理解HashMap - Java综合 - Java - ITeye论坛](http://www.iteye.com/topic/539465)
