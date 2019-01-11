---
layout: post
title:  "深度探索C++对象模型(8)"
author: E君
date:   2016-11-29
categories: C
tags: 读书笔记
---

### Data Member的布局 ###

Nonstatic data members在class object中的排列顺序将和其被声明的顺序一样，任何中间介入的static data members都不会放进对象布局之中。static data members存放在程序的data segment中，和个别的class objects无关。

C++标准要求，在同一个access section(也就是private,public, protected等区段)中，members的排列只需符合“较晚出现的members在class object中有较高的地址”这一条件即可。也就是说，各个members并不一定得连续排列，members的alignment可能就需要填补一些bytes。

编译器还可能会合成一些内部使用的data members，以支持整个对象模型，vptr就是这样的东西，目前所有的编译器都把它安插在每一个“内含virtual function的class”的object内。

C++标准也允许编译器将多个access section之中的data members自由排列，不必在乎它们出现在class声明中的顺序。目前各个编译器都是把一个以上的access section连锁在一起，依照声明的顺序，成为一个连续区块。access sections的多寡并不会招来额外负担。

### Data Member的存取 ###

**Static Data Members**

static data members被编译器提出于class之外并被视为一个global变量(但只在class生命范围之内可见)。每一个member的access section以及与class的关联并不会导致任何空间或执行时间上的额外负担(无论是个别的class objects还是static data member本身)。

每一个static data member只有一个实例，存放在程序的data segment之中，每次程序取用static member时，就会被内部转化为对该唯一extern实例的直接参考操作。

从指令执行的观点来看，这是C++语言中“通过一个指针和通过一个对象来存取member，结论完全相同”的唯一一种情况。member其实并不在class object之中，因此存取static members并不需要通过class object。

如果static data members是一个从复杂继承关系中继承而来的member，那通过指针或对象来存取也是完全相同的。程序之中对于static members还是只有唯一一个实例，其存取路径仍然是直接存取。

如果取一个static data member的地址，会得到一个指向其数据类型的指针，而不是一个指向其class member的指针，因为static member并不内含在一个class object之中。

如果两个class每个都声明了一个static member并有一样的名称，那么当它们都被放在程序的data segment时，就会导致名称冲突。编译器的解决方法是暗中对每一个static data member编码，以获得一个独一无二的程序识别码。通常需要做到下面两点：

1. 一个算法，推导出独一无二的名称。
2. 万一编译系统(或环境工具)必须和使用者交谈，那些独一无二的名称可以轻易被推导回到原来的名称。

**Nonstatic Data Members**

nonstatic data members直接存放在每一个class object之中，除非经由显示或隐式的class object，否则没有办法直接存取他们。只要在一个member function中直接处理一个nonstatic data member，所谓的隐式class object就会发生。表面上所看到的对于nonstatic data member直接存取，实际上是经由一个隐式class object(由this指针表达)完成的。

想要对一个nonstatic data member进行存取操作，编译器需要把class object的起始地址加上data member的偏移位置(offset)。每一个nonstatic data member的offset在编译期即可得知，甚至如果member属于一个base class subobject(派生自单一或多重继承串链)也是一样的，因此，存取一个nonstatic data member的效率和存取一个C struct member或者一个nonderived class的member效率是一样的。

通过object存取和通过pointer存取有时候会有差异，当一个derived class的继承结构中有一个virtual base class，并且被存取的member是一个从该virtual base class继承而来的member时。这时候我们不能够说pointer必然指向哪一种class type(因此也就不知道编译期这个member真正的offset位置)，所以这个存取操作必须延迟至执行期，经过一个额外的间接导引，才能够解决。但如果使用object就不会有这些问题，其member的offset位置在编译期就固定了。