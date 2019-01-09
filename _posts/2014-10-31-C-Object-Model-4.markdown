---
layout: post
title:  "深度探索C++对象模型(4)"
categories: C
tags: "读书笔记" 
---

### 复制构造函数 ###

三种情况下，会以一个object的内容作为另一个class object的初值：

- 对一个object做显式的初始化操作
- 当object被当作参数传递给某个函数时
- 当函数返回一个class object时

### Default Memberwise Initialization ###

如果class没有提供一个explicit copy constructor，内部以default emberwise initialization完成复制构造，即把每一个内建的或派生的data member的值，从某个object拷贝一份到另一个object身上。不过它并不会拷贝其中的member class object，而是以递归的方式施行memberwise initialization。

就像default constructor一样，C++ Standard上说，如果class没有声明一个copy constructor，就会有隐式的声明(implicitly declared)或隐式的定义(implicitly defined)出现。和以前一样，C++ Standard把copy constructor区分为trivial和nontrivial两种。只有nontrivial的实体才会被合成于程序之中。决定一个copy constructor是否为trivial的标准在于class是否展现出所谓的“bitwise copy semantics”。

### Bitwise Copy Semantics ###

class通常会进行bitwise copy semantics，有四种情况下不会出现bitwise copy semantics(想想什么时候会合成一个default constructor以及copy constructor、operator=和destructor之间的关系)：

- 当class内含一个member object而后者声明有一个copy constructor时。
- 当class继承自一个base class而后者存在一个copy constructor时。
- 当class声明了一个或多个functions时。
- 当class派生自一个继承串链，其中有一个或多个virtual base classes时。

前两种情况下，编译器必须将member或base class的copy constructor调用操作安插到被合成的copy constructor中（类似合成default constructor）。

### 重新设定virtual table的指针 ###

假设class声明了一个或多个virtual function，编译期间会进行程序扩张操作：
- 增加一个virtual function table，内含每一个有作用的virtual function的地址
- 将一个指向virtual function table的指针(vptr)，安插在每一个class object中

显然，如果编译器对于每一个新产生的class object的vptr不能成功而正确地设好其初值，将导致错误的结果。因此，当编译器导入一个vptr到class中时，该class就不再展现bitwise semantics了。现在，编译器需要合成出一个copy constructor，以便将vptr适当地初始化。

**当一个base class object以其derived class object做初始化操作时，其vptr复制也必须保证安全。**

### 处理Virtual Base Class Subobject ###

一个class object如果以另一个object作为初值，而后者有一个virtual base class subobject，那么也会使bitwise copy semantics失效。

每一个编译器对虚拟继承的支持承诺，都表示必须让derived class object中的virtual base class subobject位置在执行期就准备妥当。维护“位置的完整性”是编译器的责任。“bitwise copy semantics”可能会破坏这个位置，所以编译器必须在它自己合成出来的copy constructor中做出仲裁。

**问题并不发生在一个class object以另一个同类的object作为初值，而是发生在一个class object以其derived classes的某个object作为初值。**