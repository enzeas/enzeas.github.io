---
layout: post
title:  "深度探索C++对象模型(9)"
author: E君
date:   2016-12-2
categories: C
tags: 读书笔记
---

### Data Member与Inheritance ###

在C++继承模型中，一个derived class object所表现出来的东西，是其自己的members加上其base classes members的总和。至于derived class members和base classes members的排列顺序，则并未在C++标准中强制指定，在大部分编译器上base class members总是先出现，但属于virtual base class的可能会有例外。

**没有多态的继承(Inheritance without Polymorphism)**

一般而言，具体继承(concrete inheritance,相对于virtual inheritance)并不会增加空间或存取时间上的额外负担。
<pre>
class Point2d
{
public:
	float x,y;
};
class Point3d: public Point2d
{
public:
	float z;
};
</pre>
通过派生来设计将会使得数据实例和操作方法均进行了继承，并将其局部化。此外，这个设计可以明显表现出两个抽象类之间的紧密关系，所以这两个抽象类的使用者不需要知道objects是否为独立的classes类型，或是彼此之间有继承的关系。

把两个原本就独立不相干的classes凑成一对“type/subtype”并带有继承关系，可能会让人重复设计相同操作的函数。而把一个class分解成两层或者更多层，有可能为了表现class体系的抽象化而导致其空间膨胀(对齐)。
<pre>
Point1d {char x;};
Point2d: public Point1d {char y;};
Point3d: public Point2d {char z;};
Point {char x,y,z;};
//此时Point3d大小为12，而Point大小为4
</pre>

**加上多态(Adding Polymorphism)**

如果需要在继承关系中提供一个virtual function接口，那么将会导致空间和存取时间上的额外负担。
<pre>
void fun(Point2d &p1， Point2d &p2);
//这里p1和p2可能是Point2d也可能是Point3d
</pre>
- 导入一个virtual table，用来存放它所声明的每一个virtual functions的地址，这个table的元素个数一般而言是被声明的virtual functions的个数，再加上一个或两个slots(用以支持runtime type identification)。
- 在每一个class object中导入一个vptr，提供执行期的链接，使得每一个object能够找到相应的virtual table。
- 加强constructor，使得它能够为vptr设定初值，让它指向class所对应的virtual table。这可能意味着在derived class和每一个base class的constructor中，重新设定vptr的值。
- 加强destructor，使得它能够抹去指向class相关virtual table的vptr。vptr很可能已经在derived class destructor中被设定为derived class的virtual table地址。destructor的调用顺序是反向的，从derived class到base class。

虽然class的声明语法没有改变，但可能一些member functions变成了virtual functions，每一个derived class内都内含一个额外的vptr member，多了一个virtual table，此外每一个virtual member function的调用也比以前复杂了。

vptr在某些编译器中被放在class object的尾端，可以保留base class C struct的对象布局，因而允许在C程序代码中也能使用。某些编译器(VC)把vptr放在class object的前端，对于在多重继承之下通过指向class members的指针调用virtual function会来带一些帮助。

**多重继承(Multiple Inheritance)**

多重继承的问题主要发生于derived class objects和其第二或后继的base class objects之间的转换。对于一个多重派生对象，将其地址指定给第一个base class的指针，情况将和单一继承时相同，因为两者都指向相同的起始地址，需要的成本只有该地址的指定操作。至于第二个或后继的base class的地址指定操作，则需要将地址修改过，加上或减去介于中间的base class subobjects的大小。

C++标准并未要求多重继承中的base classes之间有特定的排列顺序。目前大多数编译器根据声明顺序来排列它们完成多重base classes的布局。

如果要存取第二个或后继base class中的一个data member，只需要一个简单的offset运算，就像单一继承一样简单。

**虚拟继承(Virtual Inheritance)**

class如果内含一个或多个virtual base class subobjects，将被分割为两部分，一个不变区域和一个共享区域。不变区域中的数据，不管后继如何衍化，总是拥有固定的offset，所以这一部分的数据可以被直接存取。至于共享区域，所表现的就是virtual base class subobject，这一部分数据其位置会因每次派生操作而有变化，所以它们只可以被间接存取。各家编译器实现技术之间的差异就在于间接存取的方法不同。

一般的布局策略是先安排好derived class的不变部分，然后再建立共享部分。编译器会在每一个derived class object中安插一些指针，每个指针指向一个virtual base class，要存取继承得来的virtual base class members，可以通过相关指针间接完成。这样的实现模型主要有两个缺点：

1. 每一个对象必须针对其每一个virtual base class背负一个额外的指针，然而理想上希望class object有固定的负担，不因为其virtual base的个数而有所变化。 
2. 由于虚拟继承串链的加长，导致间接存取层次的增加(有几层虚拟派生就要就有几个virtual base class指针存取)，然而理想上希望有固定的存取时间，不因为虚拟派生的深度而改变。

- 对于第一个问题，通常有两个解决方法。可以引入所谓的virtual base class table，每一个class object如果有一个或多个virtual base classes，就会由编译器安排一个指针，指向virtual base class table，至于真正的virtual base class指针也放在该表格中。也可以在virtual function table中放置virtual base class的offset，将virtual base class offset和virtual function entries混杂在一起，virtual function table可以经由正值或负值来索引(分别索引到virtual functions和virtual base class offsets)。

**一般而言，virtual base class最有效的一种运用形式就是：一个抽象的virtual base class，没有任何的data members(Interface)。**
