---
layout: post
title:  "R基础(3)"
categories: R
tags: "读书笔记" 
---

### 多维数组和矩阵的构造 ###

- **将向量定义成数组**
<pre>
#dim(x)取出或设置对象的维度
z = 1:12
dim(z) = c(3,4)
#将向量定义维数就成为一个数组
dim(z) = 12
#将向量定义为一个一维数组
</pre>
- **array()**
<pre>
#array(data = NA, dim = length(data), dimnames = NULL)
X = array(1:20,dim=c(4,5))
Z = array(0, dim=c(3,4,2))
#定义一个3*4*2的数组，其中元素均为0
</pre>
- **matrix()**
<pre>
#matrix(data = NA, nrow = 1, ncol = 1, byrow = FALSE, dimnames = NULL)
A = matrix(1:15, nrow=3, ncol=5, byrow=TRUE)
A = matrix(1:15, ncow=3, byrow=TRUE)
A = matrix(1:15, ncol=5, byrow=TRUE)
#取消设置byrow=TRUE数据将会按列设置
</pre>

### 下标的使用 ###

- **数组下标**
<pre>
a = 1:24
dim(a) = c(2,3,4)
a[2,1,2]
#访问数组的某个元素
a[1,2:3,2:3]
#访问数组的多个元素，下标使用列表
a[1, , ]
#忽略某维下标表示该维全选
a[] = 0
#a[ , , ]或者a[]表示整个数组
a[3:10]
#只使用一个下标向量时忽略数组的维数信息，看作对数组的数据向量取子集
</pre>
- **不规则的数组下标**
<pre>
b = matrix(c(1,1,1,2,2,3,1,3,4,2,1,4), ncol=3, byrow=T)
a[b]
#此时b是一个矩阵，按矩阵b的每行作为下标进行整体访问
#取出的是一个向量，可以进行赋值等操作
</pre>

### 数组的四则运算 ###

<pre>
A = matrix(1:6, nrow=2, byrow=T)
B = matrix(1:6, nrow=2)
C = matrix(c(1,2,2,3,3,4), nrow=2)
D = 2*C+A/B
#加减法运算和数乘运算满足矩阵运算的性质
#乘除法运算是对数组中对应位置的元素做运算
x1 = c(100,200)
x2 = 1:6
x1 + x2
#将向量中的数据与对应向量中的数据进行运算，将短向量的数据循环使用，从而可以与长向量进行匹配，并尽可能保留共同的向量属性
x3 = matrix(1:6, nrow=3)
x1 + x3
#当向量与数组运算时，向量按列匹配
</pre>

### 矩阵的运算 ###

- **转置**
<pre>
A = matrix(1:6, nrow=2)
t(A)
#t(A)表示求矩阵A的转置
</pre>
- **行列式**
<pre>
det(matrix(1:4,nrow=2))
#det(A)求矩阵A的行列式的值
</pre>
- **内积**
<pre>
x = 1:5
y = 2*1:5
x %*% y
#若x与y是相同维的向量，x %*% y表示向量x与y求内积
#crossprod(x,y)表示x与y的内积
#crossprod(x)表示x与x的内积，即x的模
</pre>
- **外积**
<pre>
x %o% y
#x %o% y表示向量x与y求内积
#outer(x,y)表示x与y的外积
#tcrossprod(x)表示x%*%t(y)，即外积
</pre>
- **矩阵乘法**
<pre>
A = array(1:9, dim=c(3,3))
B = array(9:1, dim=c(3,3))
C = A * B
D = A %*% B
# crossprod(A,B) 表示 t(A) %*% B
# tcrossprod(A,B) 表示 A %*% t(B)
</pre>
- **对角矩阵**
<pre>
#diag(x = 1, nrow, ncol)
#diag(x) <- value
v = c(1,4,5)
dig(v)
#对向量使用时表示以向量为对角线元素的对角阵
M = array(1:9, dim=c(3,3))
dig(M)
#对矩阵使用时表示取矩阵对角线上元素的向量
</pre>
- **线性方程组与逆矩阵**
<pre>
#solve(a, b, tol, LINPACK = FALSE, ...)
#a为一个方阵，b为一个向量
A = t(array(c(1:8,10),dim=c(3,3)))
b = c(1,1,1)
x = solve(A,b)
#solve(A,b)即为求方程组Ax=b的解
B = solve(A)
#solve(A)即为求矩阵A的逆
</pre>
- **特征值与特征向量**
<pre>
#eigen(x, symmetric, only.values = FALSE, EISPACK = FALSE)
sm = crossprod(A,A)
ev = eigen(sm)
#eigen(sm)求对称矩阵的特征值与特征向量，由列表形式给出
#ev$values是特征值构成的向量，ev$vectors是特征向量构成的矩阵
</pre>
- **奇异值分解**
<pre>
#svd(x, nu = min(n, p), nv = min(n, p), LINPACK = FALSE)
svdA = svd(A)
svdA$u %*% diag(svdA$d) %*% t(svdA$v)
#svd(A)返回值是一个列表，其中svd(A)$u表示矩阵A的奇异值，svd(A)$u对应正交阵U，svd(A)$v对应正交阵V
</pre>
- **最小二乘拟合**
<pre>
#lsfit(x, y, wt = NULL, intercept = TRUE, tolerance = 1e-07, yname = NULL)
x = c(0.0, 0.2, 0.4, 0.6, 0.8)
y = c(0.9, 1.9, 2.8, 3.3, 4.2)
ls.sol = lsfit(x,y)
#lsfit(x,y)返回值是一个列表，lsfit(x,y)$coefficients是拟合系数，lsfit(x,y)$residuals是拟合残差
ls.diag(ls.sol)
#ls.diag(ls.out)给出进一步的结果
</pre>
- **QR分解**
<pre>
#qr(x, tol = 1e-07 , LAPACK = FALSE, ...)
X = matrix(c(rep(1,5),x),ncol=2)
Xplus = qr(X)
#qr(x)返回值是一个列表，其中qr(x)$qr是一个与x有相同维数的矩阵，上三角储存的是R，下三角储存的是Q
b = qr.coef(Xplus,y)
fit = qr.fitted(Xplus,y)
res = qr.resid(Xplus,y)
#计算最小二乘的系数，拟合值和残差值
</pre>

### 相关函数 ###

- **维数**
<pre>
dim(A)
nrow(A)
ncol(A)
#取得矩阵A的维度，行数和列数
</pre>
- **合并**
<pre>
#cbind(..., deparse.level = 1)
#rbind(..., deparse.level = 1)
x1 = rbind(c(1,2),c(3,4))
x2 = 10+x1
x3 = cbind(x1,x2)
x4 = rbind(1,x2)
#将向量、矩阵或数据框通过列或者行组合成一个大矩阵
#如果参与合并的变量长度不同，则循环补足后合并
</pre>
- **拉直**
<pre>
A = matrix(1:6, nrow=2)
as.vector(A)
#使用as.vector(A)可以将矩阵转化为向量
</pre>
- **名字**
<pre>
X = matrix(1:6, ncol=2, dimnames=list(c("one","two","three"), c("First","Second")), byrow=T)
#数组有一个属性dimnames保存各个下标的名字，默认值为NULL
X = matrix(1:6, ncol=2, byrow=T)
dimnames(X) = list(c("one","two","three"), c("First","Second"))
#可以先定义矩阵，再为dimnames()赋值
X = matrix(1:6, ncol=2, byrow=T)
colnames(X) = c("First","Second")
rownames(X) = c("one","two","three")
#可以使用属性rownames和colnames来访问行名和列名
</pre>
- **移项**
<pre>
#aperm(a, perm = NULL, resize = TRUE, ...)
A = array(1:24, dim=c(2,3,4))
B = aperm(A, c(2,3,1))
#aperm(A,perm)把数组A的各维按perm中指定的新次序重新排列
</pre>
- **apply()**
<pre>
#apply(X, MARGIN, FUN, ...)
A = matrix(1:6, nrow=2)
apply(A,1,sum)
apply(A,2,mean)
#使用apply(A,margin,fun)对矩阵A的第margin维执行fun操作
</pre>
