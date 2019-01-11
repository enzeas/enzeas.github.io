---
layout: post
title:  "R数据描述(4)"
author: E君
date:   2015-1-5
categories: R
tags: 读书笔记
---

### 二元数据的数据描述 ###

<pre>
ore = data.frame(
        x = c(67, 54, 72, 64, 39, 22, 58, 43, 46, 34), 
        y = c(24, 15, 23, 19, 16, 11, 20, 16, 17, 13))
apply(ore,2,mean)
ore.s = cov(ore)
ore.r = cor(ore)
#cov(x, y=NULL, use="all.obs",
#    method=c("pearson","kendall","spearman"))
#cor(x, y=NULL, use="all.obs",
#    method=c("pearson","kendall","spearman"))
#x为向量、矩阵或数据框，y是空值、向量、矩阵或数据框(需与x的维数一致)
</pre>

### 二元数据的相关性检验 ###

<pre>
ruben.test = function(n, r, alpha=0.05){
	u = qnorm(1-alpha/2)
	r_star = r/sqrt(1-r^2)
	a = 2*n-3-u^2
	b = r_star*sqrt((2*n-3)*(2*n-5))
	c = (2*n-5-u^2)*r_star^2-2*u^2
	y1 = (b-sqrt(b^2-a*c))/a
	y2 = (b+sqrt(b^2-a*c))/a
	data.frame(n=n, r=r, conf=1-alpha,
               L=y1/sqrt(1+y1^2), U=y2/sqrt(1+y2^2))
}
ruben.test(6,0.8)
ruben.test(25,0.7)
#cor.test(x, y,
#         alternative = c("two.sided", "less", "greater"),
#         method = c("pearson", "kendall", "spearman"),
#         exact = NULL, conf.level = 0.95, continuity = FALSE, ...)
#cor.test(formula, data, subset, na.action, ...)
cor.test(ore$x,ore$y)
cor.test(ore$x,ore$y,method="spearman")
cor.test(ore$x,ore$y,method="kendall")
</pre>

### 多元数据的数字特征及相关矩阵 ###

<pre>
rubber = data.frame(
    X1 = c(65, 70, 70, 69, 66, 67, 68, 72, 66, 68),
    X2 = c(45, 45, 48, 46, 50, 46, 47, 43, 47, 48),
    X3 = c(27.6, 30.7, 31.8, 32.6, 31.0, 
           31.3, 37.0, 33.6, 33.1, 34.2))
apply(rubber,2,mean)
cov(rubber)
cor(rubber)
cor.test(~X1+X2,data=rubber)
cor.test(~X1+X3,data=rubber)
cor.test(~X2+X3,data=rubber)
</pre>

### 轮廓图 ###

<pre>
outline = function(x, txt=TRUE){
	if(is.data.frame(x)==TRUE)
		x = as.matrix(x)
	m = nrow(x)
	n = ncol(x)
	plot(c(1,n), c(min(x),max(x)), type="n",
		main="The outline graph of Data",
		xlab="Number", ylab="Value")
	for(i in 1:m){
		lines(x[i,],col=i)
		if(txt==TRUE){
			k = dimnames(x)[[1]][i]
			text(1+(i-1)%%n, x[i,1+(i-1)%%n], k)
		}
	}
}
X = data.frame(
	X1 = c(99, 99, 100, 93, 100, 90, 75, 93, 87, 95, 76, 85),
	X2 = c(94, 88, 98, 88, 81, 78, 73, 84, 73, 82, 72, 75),
	X3 = c(93, 96, 81, 88, 72, 82, 88, 83, 60, 90, 43, 50),
	X4 = c(100, 99, 96, 99, 96, 75, 97, 68, 76, 62, 67, 34),
	X5 = c(100, 97, 100, 96, 78, 97, 89, 88, 84, 39, 78, 37))
outline(X)
</pre>

### 星图 ###

<pre>
#stars(x, full = TRUE, scale = TRUE, radius = TRUE,
#      labels = dimnames(x)[[1]], locations = NULL,
#      nrow = NULL, ncol = NULL, len = 1,
#      key.loc = NULL, key.labels = dimnames(x)[[2]],
#      key.xpd = TRUE,
#      xlim = NULL, ylim = NULL, flip.labels = NULL,
#      draw.segments = FALSE,
#      col.segments = 1:n.seg, col.stars = NA, col.lines = NA,
#      axes = FALSE, frame.plot = axes,
#      main = NULL, sub = NULL, xlab = "", ylab = "",
#      cex = 0.8, lwd = 0.25, lty = par("lty"), xpd = FALSE,
#      mar = pmin(par("mar"),
#                 1.1+ c(2*axes+ (xlab != ""),
#                 2*axes+ (ylab != ""), 1, 0)),
#      add = FALSE, plot = TRUE, ...)
stars(X)
stars(X, full=F, draw.segments=T, key.loc=c(5,0.5), mar=c(2,0,0,0))
</pre>

### 调和曲线图 ###

<pre>
unison = function(x){
	if(is.data.frame(x)==TRUE)
		x = as.matrix(x)
	t = seq(-pi, pi, pi/30)
	m = nrow(x)
	n = ncol(x)
	f = array(0, c(m, length(t)))
	for(i in 1:m){
		f[i,] = x[i,1]/sqrt(2)
		for(j in 2:n){
			if(j%%2==0)
				f[i,] = f[i,]+x[i,j]*sin(j/2*t)
			else
				f[i,] = f[i,]+x[i,j]*cos(j%%2*t)
		}
	}
	plot(c(-pi,pi), c(min(f),max(f)), type="n",
		main="The Unision graph of Data",
		xlab="t", ylab="f(t)")
	for(i in 1:m)
		lines(t, f[i,], col=i)
}
unison(X)
</pre>