---
layout: post
title:  "深度探索C++对象模型(7)"
categories: C
tags: "读书笔记" 
---

### Data语意学 ###

一个空的class如
<pre>
class X {};
</pre>
事实上并不是空的，他有一个隐藏的1 byte大小，是被编译器安插进去的一个char。这样使得这一class的两个objects得以在内存中配备独一无二的地址。
一个空的derived class如
<pre>
class Y: public virtual X {};
class Z: public virtual X {};
</pre>
也不是空的，它的大小和机器有关，也和编译器有关。主要包括：

1. **语言本身造成的额外负担(overhead)** 当语言支持virtual base classes时，就会造成一些额外负担。在derived class中，这个额外的负担表现在某种形式的指针上，它或者指向virtual base class subobject，或者指向一个相关表格(表格中要么存放的是virtual base class subobject的地址，要么是其偏移地址offset)。
2. **编译器对于特殊情况所提供的优化处理** virtual base class X subobject的1 bytes大小也出现在class Y身上。通常它被放在derived class的固定部分的尾端。某些编译器会对empty virtual base class提供特殊支持。
3. **alignment的限制** class Y的大学目前为5 bytes(32bit机器)。在大部分机器上，聚合结构体大小会受到alignment的限制，使得它们能够更有效率地在内存中被存取。

empty virtual base class是C++面向对象上经常使用的一个术语，通常提供一个virtual interface，没有定义任何数据(类似于C#中的interface，接口)。某些新的编译器对其实现了特殊处理，一个empty virtual base class被视为derived class object最开头的一部分，也就是说并没有花费任何的额外空间。在此模型下Y的大小是4而不是8。

那么一个multiple and virtual inheritance class如
<pre>
class A: public Y, public Z {};
</pre>
由于一个virtual base class subobject只会在derived class中存在一份实例，不管它在class继承体系中出现了多少次。因此A的大小为：

- 被大家共享的唯一一个class X实例，大小为1 byte。
- base class Y的大小，减去因virtual base class X而配置的大小，结果是4 bytes。
- base class Z的大小，减去因virtual base class X而配置的大小，结果亦是4 bytes。
- class A自己的大小0 byte。
- class A的alignment数量(如果有的话)。前三项总和表示调整前的大小是9 bytes。class A必须调整至4 bytes边界，所以需要填补3 bytes。结果为12bytes。

某些编译器如果实现了特殊处理的话，此时class X的那1 byte将被拿掉，那么也不会有额外的3 bytes的alignment，因此class A的大小将是8 bytes。如果在virtual base class X中放置一个或以上的data members，两种编译器就会产生出相同的对象布局。

C++对象模型尽量以空间优化和存取速度优化的考虑来实现nonstatic data members，并保持和C语言struct数据配置的兼容性。它把数据直接存放在每一个class object之中，对于继承而来的nonstatic data members(不管是virtual还是nonvirtual base class)也是如此，不过并没有强制定义其间的排列顺序。至于static data members，则被放置在程序的一个global data segment中，不会影响个别的class object的大小。在程序中，不管该class被产生出多少个objects(经由直接产生或间接派生)，static data members永远只存在一份实例。

每一个class object因此必须有足够的大小以容纳它所有的nonstatic data members，有时候其值可能比想象中的还要大，原因是：

1. 由编译器自动加上额外的data members，用以支持某些语言特性(主要是各种virtual特性)。
2. 因为alignment(边界调整)的需要。