---
layout: post
title:  "Markdown插入公式"
author: E君
date:   2018-5-6
categories: 计算机
tags: 技巧
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<pre>
&lt;script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"&gt;&lt;/script&gt;
</pre>

- 行间插入
<pre>\\(a + b\\)</pre>
\\(a + b\\)

- 另取一行
<pre>$$ a + b $$</pre>
$$ a + b $$

- 上、下标
<pre>x_{sub}^{sup}</pre>

$$x_1$$

$$x_1^2$$

$$x^2_1$$

$$x_{22}^{(n)}$$

$${}^*x^*$$

$$x_{balabala}^{bala}$$

$$\frac{x+y}{2}$$

$$\frac{1}{1+\frac{1}{2}}$$

- 分式
<pre>frac{}{}</pre>

$$\frac{x+y}{2}$$

$$\frac{1}{1+\frac{1}{2}}$$

- 根式
<pre>sqrt[]{}</pre>

$$\sqrt{2}<\sqrt[3]{3}$$

$$\sqrt{1+\sqrt[p]{1+a^2}}$$

$$\sqrt{1+\sqrt[^p\!]{1+a^2}}$$

- 求和、积分 
<pre>sum_{s}^{e}exp
int_{a}^{b} f(x)dx</pre>

$$\sum_{k=1}^{n}\frac{1}{k}$$

$$\int_a^b f(x)dx$$

- 空格
<pre>\ \quad </pre>

紧贴 $$a\!b$$
没有空格 $$ab$$
小空格 $$a\,b$$
中等空格 $$a\;b$$
大空格 $$a\ b$$
quad空格 $$a\quad b$$
两个quad空格 $$a\qquad b$$

- 公式界定符
<pre>\left \right</pre>

$$ ( $$
$$ ) $$
$$ [ $$
$$ ] $$
$$ \{ $$
$$ \} $$
$$ | $$
$$ \| $$

$$\left(\sum_{k=\frac{1}{2}}^{N^2}\frac{1}{k}\right)$$

- 矩阵
<pre>\begin{matrix} \end{matrix}
& 区分行间元素，\\\\ 代表换行</pre>

$$\begin{matrix}1 & 2\\\\3 &4\end{matrix}$$

$$\begin{pmatrix}1 & 2\\\\3 &4\end{pmatrix}$$

$$\begin{bmatrix}1 & 2\\\\3 &4\end{bmatrix}$$

$$\begin{Bmatrix}1 & 2\\\\3 &4\end{Bmatrix}$$

$$\begin{vmatrix}1 & 2\\\\3 &4\end{vmatrix}$$

$$\left|\begin{matrix}1 & 2\\\\3 &4\end{matrix}\right|$$

$$\begin{Vmatrix}1 & 2\\\\3 &4\end{Vmatrix}$$

- 排版数组
<pre>\ldots \vdots \ddots</pre>

$$
\mathbf{X} =
\left( \begin{array}{ccc}
x\_{11} & x\_{12} & \ldots \\\\
x\_{21} & x\_{22} & \ldots \\\\
\vdots & \vdots & \ddots
\end{array} \right)
$$

- 参考

[http://www.cnblogs.com/houkai/p/3399646.html](http://www.cnblogs.com/houkai/p/3399646.html)

[https://juejin.im/post/5a6721bd518825733201c4a2](https://juejin.im/post/5a6721bd518825733201c4a2)