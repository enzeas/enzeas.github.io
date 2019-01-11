---
layout: post
title:  "深度探索C++对象模型(6)"
author: E君
date:   2016-11-19
categories: C
tags: 读书笔记
---

### 成员初始化列表 ###

**Member Initialization List**

设定class members的初值一般在member initialization list或者constructor本体之内。除了这四种情况外，这两种选择其实差不多：

1. 当初始化一个reference member。
2. 当初始化一个const member。
3. 当调用一个base class 的constructor，而它拥有一组参数的时。
4. 当调用一个member class 的constructor ,而它拥有一组参数时。

这四种情况下，程序可以被正确编译并执行，但是效率不高(此时最好使用member initialization list)。

编译器会一一操作initialization list，以适当的顺序在constructor中插入初始化操作，并且保证它们都发生在任何explicit user code之前。

**member object的初始化，最好放到初始化列表里面。**

若放置于constructor中，则可能会产生临时的object，使用default constructor初始化，再使用operator=()赋值运算给object，然后临时object自行调用destructor，降低程序效率。若置于initialization list中，则编译器会在constructor中，user code之前，调用object的constructor进行初始化。

**初始化次序和member在类中的声明次序一致，而与initialization list中的次序无关。**

相互关联的member，需要留意initialization list中，其中依赖的次序。

通常把其中一部分使用初始化列表初始化，而另一部分放置到constructor中使用user code予以表达，这样即便次序存在依赖，也会先执行member的constructor，再执行user code。

**member function的调用**

在constructor中调用member function是合法的,但却可能是不安全的。
因为和此object相关的this指针已经被构建妥当，然而在derived class中调用virtual function时可能是不安全的。