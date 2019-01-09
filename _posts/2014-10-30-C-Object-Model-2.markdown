---
layout: post
title:  "深度探索C++对象模型(2)"
categories: C
tags: "读书笔记" 
---
### 程序设计范式 ###

**paradigm**

A model or example of the environment and methodology in which systems and software are developed and operated. For one operational paradigm there could be several alternative development paradigms. Examples are functional programming, logic programming, semantic data modeling, algebraic computing, numerical computing, object oriented design, prototyping, and natural language dialogue.
一种环境设计和方法论的模型或范例：系统和软件以此模型来开发和运行。一个现役的范式可能会有数个开发中的替代范式。

函数化程序设计、逻辑程序设计、语意数据模型、几何计算、数值计算、面向对象设计、原型设计、自然语言。

1. 程序模型 **procedural model**
2. 抽象数据类型模型 **abstract data type model(ADT)**
3. 面向对象模型 **object-oriented model**

### C++对多态的支持 ###

1. 经由一组隐式的转化操作。例如把一个derived class指针转化为一个指向其public base type的指针。
2. 经由virtual function机制。
3. 经由dynamic_cast和typeid运算符。

主要用途是经由一个共同的接口来影响类型的封装，这个接口通常被定义在一个抽象的base class中，这个共享的接口是以virtual function机制引发的，它可以在执行期根据object的真正类型解析出到底是哪一个函数实例被调用。

### class object的内存大小 ###

1. non-static data members的总和大小。
2. 加上任何由于alignment(对齐)的需求而padding(填补)上去的空间（可能存在于members之间，也可能存在于集合体边界）。
3. 加上为了支持virtual而由内部产生的任何额外负担(overhead)。

### 指针的类型 ###

以内存需求的观点来说，指向一个object的指针与指向一个int的指针没有什么不同，都需要有足够的内存来放置一个机器地址(word)。“指向不同类型的指针”之间的差异，既不在其指针表示法不同，也不在其内容（代表一个地址）不同，而是在其所寻址出来的object类型不同。也就是说，“指针类型”会教导编译器如何解释某个特定地址中内存内容及其大小。

一个类型为void*的指针只能持有一个地址，而不能通过它操作所指的object。

转换(cast)是一种编译器指令，大部分情况下它并不改变一个指针所含的真正地址，它只影响“所指出的内存大小和其内容”的解释方式。

### 其他 ###

编译器将决定固定的可用接口和该接口的access level(public,private,public)。

当一个base class object初始化时直接指定为一个derived class object时，derived class将会被切割以放入较小的base type内存中，不再呈现多态。

- **OB** (object-based)
- **OO** (object-oriented)

一个OB设计可能比一个对等的OO设计速度更快且空间更紧凑。

- 速度快是因为所有的函数调用操作都在编译时期解析完成，对象建构起来时不需要设置virtual机制
- 空间紧凑是因为每一个class object不需要负担传统上为了支持virtual机制而需要的额外负荷。