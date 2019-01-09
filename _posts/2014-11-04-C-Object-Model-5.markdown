---
layout: post
title:  "深度探索C++对象模型(5)"
categories: C
tags: "读书笔记" 
---

### 程序转化语义学 ###

**Program Transformation Semantics**

主要讲编译器对用户程序的处理以进行优化及处理的规则。

### 显示初始化(Explicit Initialization) ###

当用户使用显示初始化的时候，编译器会进行对象重新定义，并调用对应的拷贝构造函数。

1. 重写每一个定义，其中的初始化操作会被剥除。
2. class的copy constructor调用操作会被安插进去。

### 参数初始化(Argument Initialization) ###

当函数调用参数是class object的时候，那么编译器可能会做如下两种不同的操作：

操作一：导入临时object，调用copy constructor将它初始化，重新改写函数调用操作，并将函数声明进行转化(从原来的class object改成class reference)。

操作二：把实际参数直接建构在其应该的位置上，此位置视函数活动范围的不同，记录在程序堆栈上。在函数返回之前，local object的destructor会被执行。

### 返回值初始化(Return Value Initialization) ###

函数需要返回一个class object时，编译器通常做一个转化：

1. 首先加上一个额外参数，类型是class object的一个reference，这个参数将用来放置copy constructed而得的返回值。
2. 在return之前安插一个copy constructor调用操作，以便将欲传回的object内容当做上述新增参数的初值。

### 使用者初始化(Optimization at the User Level) ###

基于返回值初始化，定义一个计算用的constructor(新增一些constructor)。

为了效率而生，然而这样会降低抽象化。

### 编译器初始化(Optimization at the Compiler Level) ###

编译器初始化和返回值初始化的编译器处理方式一样。这样的编译器优化操作有时称为Named Return Value(NRV)优化。

通常需要定义一个copy constructor来激活编译器中的NRV优化。NRV优化的执行并不通过另外的独立优化工具完成。

- 优化由编译器默默完成，而它是否真的被完成却不十分清楚。
- 一旦函数变得比较复杂，优化也就变得比较难以执行。
- 有时候不需要应用程序被优化。

优点：导致机器码产生时有明显的效率提升。
缺点：不能安全地规划copy constructor的副作用，必须视执行而定。

### 是否需要copy constructor ###

举个栗子
<pre>
class Point3d
{
public:
	Point3d(float x, float y, float z);
private:
	float _x, _y, _z;
};
</pre>
不需要提供一个explicit copy constructor。它既没有任何member class objects(或base class objects)带有copy constructor，也没有任何的virtual base class或virtual function。因此它的memberwise initialization操作将会导致bitwise copy。这样的效率很高，而且bitwise copy既不会导致memory leak，也不会导致address aliasing，同样安全。编译器已经自动实施了最好的行为。

如果提供一个inline copy constructor，那么编译器就会实现NRV优化：
<pre>
Point3d::Point3d(const Point3d &rhs)
{
	_x = rhs._x;
	_y = rhs._y;
	_z = rhs._z;
}
</pre>
如果使用C++ library的memcpy()会更有效率：
<pre>
Point3d::Point3d(const Point3d &rhs)
{
	memcpy(this, &rhs, sizeof(Point3d));
}
</pre>
然而Point3d声明一个或多个virtual functions，或者内含一个virtual base class，那么使用memcpy()或者memset()将会导致那些编译器产生的内部members的初值被改写(主要是vptr)。