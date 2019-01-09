---
layout: post
title:  "R数据描述(2)"
categories: R
tags: "读书笔记" 
---

### 分布函数 ###

- 概率密度函数(d)：返回在该值的概率密度函数
- 分布函数(p)：返回在该值的累积概率函数
- 分布函数的反函数(q)：返回累积概率后的分位点
- 产生随机数(r)：返回n个随机数构成的向量
<pre>
dnorm(0)
pnorm(0)
qnorm(0.5)
rnorm(10)
</pre>


分布				|名称		|附加参数		
----------------|-----------|-----------
beta			|beta		|shape1,shape2,ncp
binomial		|binom		|size,prob
Cauchy			|cauchy		|location,scale
chi-squared		|chisq		|df,ncp
exponential		|exp		|rate
F				|f			|df1,df2,ncp
gamma			|gamma		|shape,scale
geometric		|geom		|prob
hypergeometric	|hyper		|m,n,k
log-normal		|lnorm		|meanlog,sdlog
logistic		|logis		|location,scale
negative binomial	|nbinom	|size,prob
normal			|norm		|mean,sd
Poisson			|pois		|lambda
Student's t		|t			|df,ncp
uniform			|unif		|min,max
Weibull			|weibull	|shape,scale
Wilcoxon		|wilcox		|m,n

### 直方图 ###

<pre>
#hist(x, breaks = "Sturges",
#     freq = NULL, probability = !freq,
#     include.lowest = TRUE, right = TRUE,
#     density = NULL, angle = 45, col = NULL, border = NULL,
#     main = paste("Histogram of" , xname),
#     xlim = range(breaks), ylim = NULL,
#     xlab = xname, ylab,
#     axes = TRUE, plot = TRUE, labels = FALSE,
#     nclass = NULL, warn.unused = TRUE, ...)
v = rnorm(1000)
hist(v)
hist(v,breaks=c(seq(-4,4,0.5)))
#给出的向量包括直方图的起点、终点与组距
hist(v,breaks=16)
#给出网格的个数
hist(v,freq=F)
#统计频率而不是频数(默认为频数)
hist(v,probability=T)
#统计频率，默认为F
hist(v,right=F)
#默认为(a,b]左开右闭，设为False则[a,b)左闭右开
hist(v,density=20,angle=0)
#设置阴影线的密度和角度(默认无阴影，角度为45)
hist(v,col="red",border="blue")
#填充颜色为red，边框颜色为蓝色
hist(v,main=("Histogram of normal"),xlab=("point"),ylab=("count"))
#设置直方图的标题，x轴标签和y轴标签
hist(v,plot=F)
#设置是否绘图(默认绘图，设为False则不绘图，列出直方图的各种结果)
</pre>

### 核密度估计函数 ###

<pre>
#density(x, bw = "nrd0", adjust = 1,
#        kernel = c("gaussian", "epanechnikov", "rectangular",
#                   "triangular", "biweight",
#                   "cosine", "optcosine"),
#        weights = NULL, window = kernel, width,
#        give.Rkern = FALSE,
#        n = 512, from, to, cut = 3, na.rm = FALSE, ...)
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5,
      66.6, 64.0, 57.0, 69.0, 56.9, 50.0, 72.0)
hist(w, freq=F)
lines(density(w), col="blue")
#绘制直方图的概率密度曲线
x = 44:76
lines(x, dnorm(x,mean(w),sd(w)), col="red")
#绘制正态分布的概率密度曲线
</pre>

### 经验分布 ###

<pre>
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5,
      66.6, 64.0, 57.0, 69.0, 56.9, 50.0, 72.0)
plot(ecdf(w), verticals=T, do.p=F)
#verticals表示是否竖线(默认不画)，do.p表示是否画点处的记号(默认画)
x=44:78
lines(x,pnorm(x,mean(w),sd(w)))
#绘制正态分布的累积概率密度曲线
</pre>

### QQ图 ###

<pre>
#qqnorm(y, ylim, main = "Normal Q-Q Plot",
#       xlab = "Theoretical Quantiles", ylab = "Sample Quantiles",
#       plot.it = TRUE, datax = FALSE, ...)
#qqline(y, datax = FALSE, distribution = qnorm,
#       probs = c(0.25, 0.75), qtype = 7, ...)
#qqplot(x, y, plot.it = TRUE, xlab = deparse(substitute(x)),
#       ylab = deparse(substitute(y)), ...)
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5,
      66.6, 64.0, 57.0, 69.0, 56.9, 50.0, 72.0)
qqnorm(w)
qqline(w)
#若正态QQ图上的点近似在一条直线附近，则可认为样本数据来自正态分布总体
</pre>

### 茎叶图 ###

<pre>
#stem(x, scale = 1, width = 80, atom = 1e-08)
x = c(25, 45, 50, 54, 55, 61, 64, 68, 72, 75, 75, 
      78, 79, 81, 83, 84, 84, 84, 85, 86, 86, 86, 
      87, 89, 89, 89, 90, 91, 91, 92, 100)
stem(x)
stem(x, scale=2)
stem(x, scale=0.5)
#scale控制茎叶图的长度，width是绘图的宽度，atom是容差
</pre>

### 箱线图 ###

<pre>
#boxplot(x, ..., range = 1.5, width = NULL, varwidth = FALSE,
#        notch = FALSE, outline = TRUE, names, plot = TRUE,
#        border = par("fg"), col = NULL, log = "",
#        pars = list(boxwex = 0.8, staplewex = 0.5, outwex = 0.5),
#        horizontal = FALSE, add = FALSE, at = NULL)
#boxplot(formula, data = NULL, ..., subset, na.action = NULL)
#
boxplot(x)
#formula是公式，如y~grp，这里y是由数据构成的数值型向量
#grp是数据的分组(通常是因子)，data是数据框
#notch为
A = c(79.98, 80.04, 80.02, 80.04, 80.03, 80.03, 80.04,  
      79.97, 80.05, 80.03, 80.02, 80.00, 80.02)
B = c(80.02, 79.94, 79.98, 79.97, 79.97, 80.03, 79.95, 
      79.97)
boxplot(A, B, notch=T, names=c('A','B'), vcol=c(2,3))
</pre>

### 五数总括 ###

<pre>
#fivenum(x, na.rm = TRUE)
#返回五数总括(最小值minimum, 下四分位数lower-hinge, 
#中位数median, 上四分位数upper-hinge, 最大值maximum)
x = c(25, 45, 50, 54, 55, 61, 64, 68, 72, 75, 75, 
      78, 79, 81, 83, 84, 84, 84, 85, 86, 86, 86, 
      87, 89, 89, 89, 90, 91, 91, 92, 100)
fivenum(x)
</pre>

### 正态性检验 ###

<pre>
#执行正态性Shapiro-Wilk检验
w = c(75.0, 64.0, 47.4, 66.9, 62.2, 62.2, 58.7, 63.5,
      66.6, 64.0, 57.0, 69.0, 56.9, 50.0, 72.0)
shapiro.test(w)
v = runif(100,min=2,max=4)
#制造均匀分布的随机数
shapiro.test(v)
</pre>

### 分布拟合检验 ###

<pre>
#ks.test(x, y, ...,
#        alternative = c("two.sided", "less", "greater"),
#        exact = NULL)
#x为待检验的向量，y为原假设的数据向量或者描述原假设的字符串
x = rt(100,5)
#制造服从t(5)的随机数
ks.test(x,"pf",2,5)
</pre>