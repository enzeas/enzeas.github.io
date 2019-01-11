---
layout: post
title:  "深度探索C++对象模型(3)"
author: E君
date:   2016-11-5
categories: C
tags: 读书笔记
---

### 构造函数 ###

举个栗子
<pre>
class Foo
{
	public: int val;
	Foo *pnext;
};
</pre>
什么时候会合成出一个default constructor？当编译器需要它的时候！

被合成出来的constructer只会执行编译器所需要的行动，如不一定会将其中的data member初始化。

因此设计者可能需要显式提供一个default constructor，将两个members适当地初始化。

### 什么时候编译器会合成一个default constructor ###

**带有default constructor的member class object**

如果一个class没有任何constructor，但它内含一个member object，而后者有default constructor，那么编译器就需要为该class合成出一个default constructor。不过这个合成操作只有在constructor真正需要被调用时才会发生。

如果已经显式定义了default constructor，编译器没法合成第二个，则编译器会扩张已经存在的constructor，在其中安排一些代码，使得执行之前先调用必要的default constructor。

**带有default constructor的base class**

类似道理

**带有一个virtual function的class**

class声明(或继承)一个virtual function

class派生自一个继承串链，其中有一个或更多的virtual class

由于缺乏声明的constructors，编译器会详细记录合成一个default constructor的必要信息。编译期间将发生：

- 一个virtual function table(即vtbl)被编译器产生出来，内放class的virtual functions地址
-  在每一个class object中，一个额外的pointer member(即vptr)会被编译器合成出来，内含相关class vtabl的地址。

为了使这个机制发挥效用，编译器必须为每一个derived class object的vptr设定初值，放置适当的vtbl地址。对于class定义的每一个constructor，编译器会安排一些代码来做这样的事情。对于未声明任何constructor的classes，编译器会为它们合成一个default constructor以便正确地初始化每一个object的vptr。

**带有一个virtual base class的class**

必须使virtual base class在其每一个derived class object中的位置在执行期准备妥当，对于class所定义的每一个constructor，编译器将安插那些“允许每一个virtual base class的执行期存取操作”的代码，如果class没有声明任何construcotr，编译器必须为它合成一个default constructor。

### 常见误解 ###

1. 任何class如果没有定义default constructor，就会被合成一个来。
2. 编译器合成出来的default constructor会显式设定class内每一个data member的默认值。

<pre>
class vec
{
	int len;
	int *pvec;
public:
	vec(int);
	~vec();
};
</pre>
这个类既不会合成出一个default constructor，也不会对其中的data member初始化。
