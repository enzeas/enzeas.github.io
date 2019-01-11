---
layout: post
title:  "R基础(4)"
author: E君
date:   2014-12-12
categories: R
tags: 读书笔记
---

### 读取纯文本文件 ###

- **read.table()**
<pre>
#read.table(file, header = FALSE, sep = "", quote = "\"'",
#           dec = ".", numerals = c("allow.loss", "warn.loss", "no.loss"),
#           row.names, col.names, as.is = !stringsAsFactors,
#           na.strings = "NA", colClasses = NA, nrows = -1,
#           skip = 0, check.names = TRUE, fill = !blank.lines.skip,
#           strip.white = FALSE, blank.lines.skip = TRUE,
#           comment.char = "#",
#           allowEscapes = FALSE, flush = FALSE,
#           stringsAsFactors = default.stringsAsFactors(),
#           fileEncoding = "", encoding = "unknown", text, skipNul = FALSE)
rt = read.table("houses.data")
is.data.frame(rt)
#读入的数据是一个数据框
#file是读入数据的文件名，header=TRUE表示所读数据的第一行为变量名，sep是数据分隔的分隔符
</pre>
- **scan()**
<pre>
#scan(file = "", what = double(), nmax = -1, n = -1, sep = "",
#     quote = if(identical(sep, "\n")) "" else "'\"", dec = ".",
#     skip = 0, nlines = 0, na.strings = "NA",
#     flush = FALSE, fill = FALSE, strip.white = FALSE,
#     quiet = FALSE, blank.lines.skip = TRUE, multi.line = TRUE,
#     comment.char = "", allowEscapes = FALSE,
#     fileEncoding = "", encoding = "unknown", text, skipNul = FALSE)
w = scan("weight.data")
#读取文本数据为一个向量
inp = scan("h_w.data",list(height=0,weight=0))
is.list(inp)
#读入的文本是一个列表
X = matrix(scan("weight.data",0), ncol=5, byrow=T)
#将读入的数据存成矩阵形式
x = scan()
#从标准输入直接读入数据
#file是读入数据的文件名，what指定一个列表，其中每项类型为需要读取的类型，sep是数据分隔的分隔符
</pre>

### 读取其他格式文件 ###

<pre>
#read.csv(file, header = TRUE, sep = ",", quote = "\"", 
#         dec = ".", fill = TRUE, comment.char = "", ...)
#csv
#read.delim(file, header = TRUE, sep = "\t", quote = "\"",
#           dec = ".", fill = TRUE, comment.char = "", ...)
#txt
#read.arff(file)
#weka
#read.xport(file)
#SAS
#read.spss(file, use.value.labels = TRUE, to.data.frame = FALSE,
#          max.value.labels = Inf, trim.factor.names = FALSE,
#          trim_values = TRUE, reencode = NA, use.missings = to.data.frame)
#SPSS
</pre>

### 写数据文件 ###

- **write()**
<pre>
#write(x, file = "data",
#      ncolumns = if(is.character(x)) 1 else 5,
#      append = FALSE, sep = " ")
#x是写入的数据，可以是矩阵也可以是向量
#file是文件名，append表示在原文件上添加数据
</pre>
- **write.table()和write.csv()**
<pre>
#write.table(x, file = "", append = FALSE, quote = TRUE, sep = " ",
#            eol = "\n", na = "NA", dec = ".", row.names = TRUE,
#            col.names = TRUE, qmethod = c("escape", "double"),
#            fileEncoding = "")
#txt 纯文本
#write.csv(...)
#csv Excel
</pre>

### 分支语句 ###

- **if/else**
<pre>
#if(condition)
#	statement
#if..
#if(condition)
#	statement_1
#else
#	statement_2
#if..else..
#if(condition_1)
#	statement_1
#else if(condition_2)
#	statement_2
#else
#	statement_3
#if..else if..else
</pre>
- **switch**
<pre>
#switch(statement, list)
#statement是表达式，list是列表
switch(2,2+2,mean(1:10),rnorm(4))
#若表达式不是一个字符串那么将会转换成一个整数，如果这个整数在1到length(list)之间，则返回list中相应位置的值，否则返回NULL
switch("fruit",fruit="banana",vegetable="broccoli",meat="beef")
#若表达式是一个字符串，如果这个字符串与list中的一个变量中的名字完全匹配，则返回变量名对应的值，否则返回NULL
</pre>

### 循环语句 ###

- **for**
<pre>
#for(name in expression_1) expression_2
#name是循环变量，expr_1是一个向量表达式(通常是个序列)，expr2通常是一组表达式
n = 4; x = array(0,dim=c(n,n))
for(i in 1:n){
	for(j in 1:n){
		x[i,j] = 1/(i+j-1)
	}
}
</pre>
- **while**
<pre>
#while(condition) expression
#若condition成立，则执行表达式expression
f=1; f[2]=1; i=1
while(f[i]+f[i+1]<1000){
	f[i+2] = f[i]+f[i+1]
	i = i+1
}
</pre>
- **repeat**
<pre>
#repeat expression
#依赖break语句跳出循环
f=1; f[2]=1; i=1
repeat{
	f[i+2] = f[i]+f[i+1]
	i = i+1
	if(f[i]+f[i+1]>=1000) break
}
</pre>
- **break/next**
<pre>
#break作用为中止循环
#next作用为继续执行
</pre>

### 函数 ###

<pre>
#function( arglist ) expr
#return(value)
fzero = function(f,a,b,eps=1e-5){
	if(f(a)*f(b)>0)
		list(fail="finding root is fail!")
	else{
		repeat{
			if(abs(b-a)<eps) break
			x = (a+b)/2
			if(f(a)*f(x)<0) b=x else a=x
		}
		list(root=(a+b)/2, fun=f(x))
	}
}
#定义二分法求根函数
f = function(x){ x^3 -x -1 }
#定义非线性函数
fzero(f,1,2,1e-6)
#求区间[1,2]内的根
#uniroot(f, interval, ...,
#        lower = min(interval), upper = max(interval),
#        f.lower = f(lower, ...), f.upper = f(upper, ...),
#        extendInt = c("no", "yes", "downX", "upX"), check.conv = FALSE,
#        tol = .Machine$double.eps^0.25, maxiter = 1000, trace = 0)
uniroot(f,c(1,2))
#内置函数求一元方程根
twosam = function(y1,y2){
	n1 = length(y1)
	n2 = length(y2)
	yb1 = mean(y1)
	yb2 = mean(y2)
	s1 = var(y1)
	s2 = var(y2)
	s = ((n1-1)*s1 + (n2-1)*s2) / (n1+n2-2)
	(yb1-yb2) / sqrt(s*(1/n1 + 1/n2))
}
#计算两样本的T统计量
A = c(79.98,80.04,80.02,80.04,80.03,80.03,80.04,79.97,80.05,80.03,80.02,80.00,80.02)
B = c(80.02,79.94,79.98,79.97,79.97,80.03,79.95,79.97)
twosam(A,B)
</pre>

### 二元运算 ###

<pre>
"%!%" = function(x,y) {exp(-0.5*(x-y)%*%(x-y))}
#定义一个新的函数，能够当作二元运算符使用，类似于操作符重载
</pre>

### 默认参数 ###

<pre>
Newtons = function(fun, x, ep=1e-5, it_max=100){
	index = 0
	k = 1
	while(k<=it_max){
		x1 = x
		obj = fun(x)
		x = x-solve(obj$J, obj$f)
		norm = sqrt((x-x1)%*%(x-x1))
		if(norm<ep){
			index = 1; break
		}
		k = k+1
	}
	obj = fun(x)
	list(root=x, it=k, index=index, FunVal=obj$f)
}
#这里的变量有一个方程函数，初试变量，精度要求(默认值1e-5)，最大迭代次数(默认值100)
#函数返回一个列表，root是近似值，it是迭代次数，index是一个指标(为1表示计算成功，为0表示计算失败)，FunVal是方程在root处的函数值
funs = function(x){
	f = c(x[1]^2+x[2]^2-5, (x[1]+1)*x[2]-(3*x[1]+1))
	J = matrix(c(2*x[1], 2*x[2], x[2]-3, x[1]+1), nrow=2, byrow=T)
	list(f=f,J=J)
}
#f是所求方程的函数，J是相应的Jacobi矩阵
#输出函数值和相应的Jacobi矩阵
Newtons(funs,c(0,1))
#求方程c(x[1]^2+x[2]^2-5, (x[1]+1)*x[2]-(3*x[1]+1))=c(0,1)的解
</pre>

### 递归函数 ###

<pre>
#可以在函数内定义函数本身
area = function(f,a,b,eps=1e-6,lim=10){
	fun1 = function(f,a,b,fa,fb,a0,eps,lim,fun){
		d = (a+b)/2
		h = (b-a)/4
		fd = f(d)
		a1 = h*(fa+fd)
		a2 = h*(fd+fb)
		if(abs(a0-a1-a2)<eps||lim==0)
			return (a1+a2)
		else{
			return (fun(f,a,d,fa,fd,a1,eps,lim-1,fun) + fun(f,d,b,fd,fb,a2,eps,lim-1,fun))
		}
	}
	fa = f(a)
	fb = f(b)
	a0 = ((fa+fb)*(b-a))/2
	fun1(f,a,b,fa,fb,a0,eps,lim,fun1)
}
#f是被积函数，a,b是积分的端点，eps是积分精度要求(默认值为1e-6)，lim是对分区间的上限(默认值为10，最多等分为2^10个子区间)
f = function(x) 1/x
#定义函数
area(f,1,5)
#计算积分值
</pre>
