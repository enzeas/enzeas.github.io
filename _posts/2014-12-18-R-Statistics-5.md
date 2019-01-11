---
layout: post
title:  "R数据描述(1)"
author: E君
date:   2014-12-18
categories: R
tags: 读书笔记
---

### 位置的度量 ###

- **均值**
<pre>
#mean(x, trim = 0, na.rm = FALSE, ...)
#trim为0-0.5的小数，代表求平均值时去除的异常值
#na.rm为是否去除缺失参数(默认不去除)
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5, 66.6, 64.0,57.0,69.0, 56.9, 50.0, 72.0)
mean(w)
x = matrix(1:12, nrow=3)
mean(x)
#对矩阵或数组使用mean()函数将返回其中所有数据的平均值
apply(x,1,mean)
apply(x,2,mean)
#使用apply函数能分别计算各个维度的均值
#sum(..., na.rm = FALSE)
sum(w)
#mean(w) == sum(w)/length(w)
w[1]=750
mean(w, trim=0.1)
#去除异常值的比例为0.1
w.na = c(w, NA)
mean(w, trim=0.1, na.rm=T)
#weighted.mean(x, w, ..., na.rm = FALSE)
weighted.mean(w, 1:15)
#第二个向量是x的权重，维度须与x相同
</pre>
- **顺序统计量**
<pre>
#sort(x, decreasing = FALSE, na.last = NA, ...)
#decreasing为是否逆序(默认从小到大)
#na.last为是否保留缺失值并将缺失值放在尾端(默认不保留)
x = c(75, 64, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5)
sort(x)
sort(x, decreasing=T)
#返回一个排好序的向量
x.na = c(x, NA)
sort(x.na)
sort(x.na, na.last=T)
#order(..., na.last = TRUE, decreasing = FALSE)
#返回一个向量，每个元素是排序后该位置元素原来的位置
order(x)
#rank(x, na.last = TRUE,
#     ties.method = c("average", "first", "random", "max", "min"))
rank(x)
#返回一个向量，每个元素是原向量的秩
</pre>
- **中位数**
<pre>
#median(x, na.rm = FALSE)
#当na.rm=TRUE时可以处理带有缺失数据的向量
x = c(75, 64, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5)
median(x)
x.na = c(x, NA)
median(x.na, na.rm=T)
</pre>
- **百分位数**
<pre>
#quantile(x, probs = seq(0, 1, 0.25), na.rm = FALSE,
#         names = TRUE, type = 7, ...)
#probs为百分位数，na.rm=TRUE时可以处理带有缺失数据的向量
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5, 66.6, 64.0,57.0,69.0, 56.9, 50.0, 72.0)
quantile(w)
quantile(w, probs=seq(0,1,0.2))
#求0%,20%,40%,60%,80%,100%的百分位数
</pre>

### 分散程度的度量 ###

- **方差、标准差与变异系数**
<pre>
#var(x, y = NULL, na.rm = FALSE, use)
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5, 66.6, 64.0,57.0,69.0, 56.9, 50.0, 72.0)
var(w)
#sd(x, na.rm = FALSE)
sd(w)
#变异系数需要编写简单的程序
cv = function(x) 100*sd(x)/mean(x)
cv(w)
#校正平方和需要编写简单的程序
css = function(x) sum((x-mean(x))^2)
css(w)
#未校正平方和需要编写简单的程序
uss = function(x) sum(w^2)
uss(w)
</pre>
- **极差与标准误**
<pre>
#极差、四分位差和标准误需要编写简单的程序
R = function(x) max(x)-min(x)
R(w)
R1 = function(x) quantile(x,3/4)-quantile(x,1/4)
R1(w)
sm = function(x) sd(x)/sqrt(length(x))
sm(w)
</pre>

### 分别形状的度量 ###

- **偏度系数和峰度系数**
<pre>
data_outline = function(x){
	n = length(x)
	m = mean(x)
	v = var(x)
	s = sd(x)
	me = median(x)
	cv = 100*s/m
	css = sum((x-m)^2)
	uss = sum(x^2)
	R = max(x)-min(x)
	R1 = quantile(x,3/4)-quantile(x,1/4)
	sm = s/sqrt(n)
	g1 = (n*sum((x-m)^3))/((n-1)*(n-2)*(s^3))
	g2 = (n*(n+1)*sum((x-m)^4))/((n-1)*(n-2)*(n-3)*(s^4)) - (3*(n-1)^2)/((n-2)*(n-3))
	data.frame(N=n, Mean=m, Var=v, std_dev=s, Median=me, std_mean=sm, CV=cv, CSS=css, USS=uss, R=R, R1=R1, Skewness=g1, Kurtosis=g2, row.names=1)
}
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5, 66.6, 64.0,57.0,69.0, 56.9, 50.0, 72.0)
data_outline(w)
</pre>