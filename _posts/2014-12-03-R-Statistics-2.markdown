---
layout: post
title:  "R基础(2)"
categories: R
tags: "读书笔记" 
---

### 对象属性 ###

- **固有属性**
<pre>
mode(c(1,3,5)>5)
#返回向量的类型：logical(逻辑),numeric(数值),complex(复数),character(字符)
z = 1:9
is.numeric(z)
is.character(z)
#类似还有is.logical(),is.complex()
length(z)
#返回元素的个数
digits = as.character(z)
d = as.numeric(digits)
#进行强制类型转换
</pre>
- **修改长度**
<pre>
x = numeric()
x[3] = 15
#增加对象长度可以直接做赋值运算
y = 1:10
y = y[2*1:5]
#缩小对象长度可以给对象赋予长度短的子集
length(y) = 3
#或者给对象的长度赋值
</pre>
- **attributes()和attr()**
<pre>
x = c(apple=2.5,orange=2.1)
attributes(x)
#attributes(obj)返回obj的各特殊属性组成的列表，不包括固有属性mode和length
attr(x,"names")
#attr(obj, which)取出或设置对象的一项特殊属性
attr(x,"names") = c("apple","grapes")
attr(x,"types") = "fruit"
#attr(obj, which)同样可以定义新的属性
</pre>

### 因子 ###

- **factor()**
<pre>
#factor(x = character(), levels, labels = levels, exclude = NA, ordered = is.ordered(x), nmax = NA)
sex = c("M","F","M","M","F")
sexf = factor(sex)
#创建一个因子
sexm = factor(sex, c("M","F"))
#改变因子的水平，使得"M"优先于"F"(默认字母顺序或数字顺序)
sexmale = factor(sex,c("M","F"), c("Male","Female"))
#改变因子的水平及标签
is.factor(sexmale)
#检测对象是否为因子
as.factor(sex)
#将向量转换成因子
levels(sexf)
#获取或设置因子的水平
table(sexm)
#统计各类数据的频数
</pre>
- **tapply()**
<pre>
#tapply(X, INDEX, FUN = NULL, ..., simplify = TRUE)
#X为向量，INDEX为与X同样长度的因子，FUN为需要计算的函数
height = c(174,165,180,170,160)
tapply(height,sex,mean)
#分别求height对每个sex的mean
</pre>
- **gl()**
<pre>
#gl(n, k, length = n*k, labels = seq_len(n), ordered = FALSE)
#n为水平个数，k为重复次数，length为结果长度，labels为n维向量表示因子水平，orderd为逻辑变量表示是否有序
gl(3,5)
gl(3,1,15)
</pre>

### 列表 ###

- **构造**
<pre>
#一种对象集合，元素也由下标区分，各元素可以是任意对象，也可以是其他复杂数据类型。比如列表的一个元素也能是列表。
lst = list(name="Fred", wife="Mary", no.children=3, child.ages=c(4,7,9))
lst[[2]]
lst[[4]][2]
#每次只能引用一个元素，不允许如lst[[1:2]]等用法
lst[1:2]
#通过下标和下标范围的用法也是合法的
#两层括号返回的是列表的一个元素，一层括号返回的是列表的一个子列表
lst[["name"]]
lst[["child.ages"]]
#若指定了元素的名字，则引用时候也可以使用名字作为下标
lst$name
lst$child.ages
#也可以使用listname$elementname来引用元素
</pre>
- **修改**
<pre>
#修改将元素引用赋值即可
lst$name = "John"
#增加一项直接赋值即可
lst$income = c(1980,1600)
#删除一项将该项赋空值(NULL)
lst$child.ages = NULL
#可以使用连接函数c()连接起来，结果仍然为一列表
list.ABC = c(list.A, list.B, list.C)
</pre>

### 数据框 ###

- **生成**
<pre>
#data.frame(..., row.names = NULL, check.rows = FALSE, check.names = TRUE, stringsAsFactors = default.stringsAsFactors())
df = data.frame(
Name = c("Alice","Becka","James","Jeffrey","John"),
Sex = c("F", "F", "M", "M", "M"),
Age = c(13, 13, 12, 13, 12),
Height = c(56.5, 65.3, 57.3, 62.5, 59.0),
Weight = c(84.0, 98.0, 83.0, 84.0, 99.5)
)
#使用方法和list类似，自变量可以命名
lst = list(
Name = c("Alice","Becka","James","Jeffrey","John"),
Sex = c("F", "F", "M", "M", "M"),
Age = c(13, 13, 12, 13, 12),
Height = c(56.5, 65.3, 57.3, 62.5, 59.0),
Weight = c(84.0, 98.0, 83.0, 84.0, 99.5)
)
as.data.frame(lst)
#若一个列表的各个成分满足数据框成分的要求，可以使用as.data.frame()函数强制转换为数据框
X = array(1:6, c(2,3))
data.frame(X)
#一个矩阵可以用data.frame()转换为一个数据框
</pre>
- **引用**
<pre>
df[1:2,3:5]
#和引用矩阵元素的方法相同，可以使用下标或下标向量
df[["Height"]]
df$Height
#可以按列表引用(双层括号[[]]或者$)
names(df)
rownames(df) = c("one", "two", "three", "four", "five")
#数据框的变量名由names属性定义，此属性一定是非空的
#数据框各行的变量名可以通过rownames属性定义
</pre>
- **attach()**
<pre>
attach(df)
r = Height/Weight
#将数据框里的变量链接到内存中
detach()
#取消链接
</pre>

### 编辑列表与数据框 ###
<pre>
xnew = edit(xold)
</pre>