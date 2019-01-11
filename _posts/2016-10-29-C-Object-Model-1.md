---
layout: post
title:  "深度探索C++对象模型(1)"
author: E君
date:   2016-10-29
categories: C
tags: 读书笔记
---

### 关于对象 ###

举个栗子

在C语言中，实现一个Point3d
<pre>
	typedef struct point3d
	{
		float x;
		float y;
		float z;
	} Point3d;
</pre>
如果要进行其他的操作，比如打印，存储等，需要进行定义一个函数，或者效率一些，定义一个宏，抑或直接在程序中完成操作。

在C++中，可以采用独立的抽象数据类型(abstract data type, ADT)来实现，或者以一个双层或三次的class层次结构完成。

更进一步地，不管哪种形式，都可以被参数化，可以是坐标类型的参数化，也可以是坐标类型和坐标个数两者都参数化。

### 加上封装后的布局成本 ###

实际上并没有增加成本，就像C结构体的情况一样：

- data members直接内含在每一个class object之中
- member functions虽然含在class的声明之内，却不出现在object之中
- 每一个non-inline member function只会产生一个函数实例
- inline function会在每一个使用者身上产生一个函数实例

额外负担主要由virtual引起的，包括：

- 虚函数**virtual function**机制	用以支持一个有效率的“执行期绑定”(runtime binding)
- 虚基类**virtual base class**机制	用以实现“多次出现在继承体系中的base class有一个单一而被共享的实例”

### C++对象模式 ###

两种数据成员**class data member**

- static 静态数据成员
- non-static 非静态数据成员

三种成员函数**class member function**

- static 静态成员函数
- non-static 非静态成员函数
- virtual 虚函数
<pre>
class Point
{
public:
	Point(float xval);
	virtual ~Point();
	float x() const;
	static int PointCount();
protected:
	virtual ostream & Print(ostream &) const;
	float _x;
	static int _point_count;
};
</pre>

### C++对象模型 ###

non-static data members 被配置于每一个class object之内

static data members 被存放在个别的class object之外

static member function 和 non-static member function 被放在个别的class object之外

virtual function则以两个步骤支持

1. 每一个class产生出一堆指向virtual function的指针，放在表格之中，这个表格被称为virtual table(**vtbl**)
2. 每一个class object被安插一个指针，指向相关的virtual table。通常这个指针被称为**vptr**。vptr的设定和重置由每一个class的constructer、destructer和copy assignment运算符自动完成。每一个class所关联的*type_info* object(用以支持runtime type identification, RTTI)也经由virtual table被指出来，通常放在表格的第一个(VC)或最后一个slot(gcc)。

### 关键词所带来的差异 ###

**class struct typename**

C语言

- struct是关键字而class不是
- struct内只能定义成员变量，不能定义成员函数

C++

- 用于类时，struct几乎等价于class
- class中默认的成员访问权限为private
- struct中默认的成员访问权限为public
- 继承时class默认private继承，struct默认public继承

模板

- C模板中，类型参数前面可以使用class或typename
- class或typename之后为类型参数
- struct之后为non-type template parameter