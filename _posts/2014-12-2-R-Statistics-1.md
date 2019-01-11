---
layout: post
title:  "R基础(1)"
author: E君
date:   2014-12-2
categories: R
tags: 读书笔记
---

### 向量 ###

- **赋值**
<pre>
x <- c(1,2,3,4,5)
assign("x",c(1,2,3,4,5))
#要加双引号
c(1,2,3,4,5) -> x
y = c(x,0,x)
</pre>
- **运算**
<pre>
x = c(-1,0,2)
y = c(3,8,2)
v = 2*x+y+1
#基本相同，+1代表向量的每个分量均加1
x*y
x/y
x^2
y^x
#对应向量的每个分量做乘法、除法和开发运算
5%/%3
#%/%整数除法
5%%3
#%%求余
exp(x)
sqrt(y)
#基本初等函数如log,exp,cos,tan,sqrt等
#自变量为向量时函数返回值也是向量
</pre>
- **相关函数**
<pre>
x = c(10,6,4,7,8)
min(x)
max(x)
range(x)
#分别求向量的最小分量、最大分量和向量的范围([min(x),max(x)])
which.min(x)
which.max(x)
#表示在第几个分量找到最小(最大)值
sum(x)
prod(x)
length(x)
#求各分量之和、求各分量之积、求向量分量个数
median(x)
mean(x)
var(x)
sd(x)
#中位数、均值、方差、标准差
sort(x)
#返回按递增顺序排序的向量
</pre>

###产生序列##

- **等差序列**
<pre>
#a:b表示从a开始逐项加1(减1)直到b为止
1:10
#2*1:10表示2,4,6,8,10...20，即先求向量再乘以2
1:5-1
1:(5-1)
</pre>
- **等间隔函数**
<pre>
#seq(from=1, to=1, by=((to-from)/(length-1)), length=NULL,)
seq(1,5)
seq(1,5,by=0.5)
seq(1,5,length=9)
seq(1,by=0.5,length=9)
</pre>
- **重复函数**
<pre>
#rep(x,times)
#rep(x,length)
rep(1:3,3)
rep(1:3,length=5)
</pre>

###逻辑向量###

<pre>
z = c(TRUE, FALSE，T, F)
all(z)
#判断是否全为真
any(z)
#判断是否包含真
</pre>

###缺失数据###

<pre>
z = c(1:3, NA)
is.na(z)
#判断是否为缺失值
z[is.na(z)] = 0
#将缺失值改为0
x = c(0/1, 0/0, 1/0, NA)
is.nan(x)
#判断数据是否不确定
is.finite(x)
#判断数据是否为有限值
is.infinite(x)
#判断数据是否为无穷
is.na(x)
#不确定数据也是缺失数据，缺失数据不是不确定数据
</pre>

###字符型向量###

<pre>
#paste (..., sep = " ", collapse = NULL)
paste("Hello","World")
paste("label","1:6",sep="")
paste(1:10)
paste("Today is", date())
paste(1:5,collapse='.')
</pre>

###复数向量###

<pre>
#complex(length = 0, real = numeric(), imaginary = numeric(), modulus = 1, argument = 0)
z <- complex(real = stats::rnorm(100), imaginary = stats::rnorm(100))
Re(z)
Im(z)
Mod(z)
Arg(z)
Conj(z)
#实部，虚部，模，幅角，共轭复数
</pre>

###下标运算###

- **逻辑向量**
<pre>
x = 1:5
x < 3
x[x<3]
#取出小于3的部分分量
z = c(-1, 1:3, NA)
z[is.na(z)] = 0
#将缺失数据设0
y = z[!is.na(z)]
#取出非缺失数据并赋值
y = numeric(length(x))
y[x<0] = -x[x<0]
y[x>=0] = x[x>=0]
#设计分段函数
</pre>

- **下标的整数运算**
<pre>
v = 10:20
v[c(1,3,5,1)]
#下标可以是一个向量，此时返回一个向量，取值允许重复
v[-(1:4)]
#从向量中扣除相应的元素
</pre>

- **字符型值的下标向量**
<pre>
ages = c(Zhang=10,Wang=14,Liu=13,Li=15,Chen=12)
#在定义时直接加上名字
fruit = c(5,10,15,20)
names(fruit) = c("orange","banana","apple","peach")
#之后再加上向量名
</pre>