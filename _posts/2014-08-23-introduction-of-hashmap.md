---
layout: post
title: HashMap简介
date: 2014-08-23 20:45
author: VERYYOUNG
comments: true
categories: [Java]
---
<h3>以下内容是基于HashMap源码注视的翻译</h3>
Hash表是基于Map接口的实现，这种实现提供了所有原始map的操作，允许null value和null key。

HashMap类大致相当于HashTable,只是它是不同步，并允许使用空值。
此类对map秩序的维护不做任何保证；尤其是，它并不保证顺序随着时间的推移，将保持恒定。

此实现提供常数时间复杂度的get和put操作，假设散列函数妥善分散桶与桶之间的元素。
迭代集合视图需要的时间复杂度与HashMap实例 的 "capacity"（存储桶的数目）再加上它的key-value 映射的数量 成正比，
因此如果迭代性能重要的话，非常重要的事情是一定不要把Map的初始容量设置得太高或太低。、

HashMap 实例具有两个参数，会影响其性能： 初始容量（initialCapacity）、 加载因子（loadFactor）。
容量（capacity）是哈希表存储桶的数目，初始容量（initialCapacity）是哈希表最初的存储桶的数目。
加载因子用来衡量哈希表在其容量自动增加之前有多满。当哈希表中的条目数超出负荷因子和当前容量的乘积，哈希表刷新 （即，内部数据
重新生成的结构），新生成的哈希表中有大约两倍存储桶数。

作为一般规则，默认加载因子 (0.75） 提供了良好时间和空间成本之间的权衡取舍。更高的值减少空间开销但是增加成本的查找 （反映在大多数
HashMap 类的操作(包括put，get）。预期中的entries数应该考虑到map负载因子时设置它的初始容量，以尽量减少的数量重复操作。
如果初始容量大于最大entries除以加载因子，没有 rehash 操作会发生。

如果很多映射要存储在 HashMap 实例中，创建具有足够大的容量将会允许要比让它执行更高效地存储的映射自动重复作为种植所需的表。
请注意使用许多key有同一 {@code hashcode ()} 是肯定会慢下来哈希表的性能。以减轻影响，当key是 {@link 可比}，此类可能使用的比较顺序之间键，帮助打破关系。

请注意此实现不是 synchronized的，至少一个线程修改map结构上，它必须是外部同步。
（结构的修改是添加或删除一个或多个映射的任何操作；只不断变化的价值与一个实例已经包含键关联不是结构修改）。
这通常被通过对一些自然封装map的对象同步。

如果没有这样的对象存在，map应使用{Collections#synchronizedMap Collections.synchronizedMap}方法"wrapped"。
这最好是在创建时，以防止意外不同步的访问到的映射
 Map m = Collections.synchronizedMap(new HashMap(...));

迭代器返回的所有此类"集合视图方法"是快速失败的：如果map结构上，在之后的任何时间修改
迭代器被创建的除非通过迭代器自身的任何方式删除方法，迭代器将抛出 ConcurrentModificationException。
因此，面对并发修改，迭代器快速失败、 干净，而不是冒着危险在不确定的时间，在任意的、 非确定性行为未来。

请注意，不能保证一个迭代器的快速失败行为这是，一般来说，不可能做出任何努力保证在场的不同步并发修改。
快速失败迭代器把 ConcurrentModificationException 扔在尽最大努力的基础上。
因此，它就错了，写一个程序，这取决于异常为它的正确性： 迭代器的快速失败行为应将仅用于检测 bug。
