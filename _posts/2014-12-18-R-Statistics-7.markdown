---
layout: post
title:  "R数据描述(3)"
categories: R
tags: "读书笔记" 
---

### 绘图命令 ###

- **plot()**

plot(x,y)

其中x和y是向量，生成y关于x的散点图。

plot(x)

其中x是时间序列，生成时间序列的图形。如果x是向量，则生成x关于下标的散点图。如果x是复向量，则生成复数实部与虚部的散点图。

plot(f)

plot(f,y)

其中f是因子，y是数值向量。第一种格式生成f的直方图，第二种格式生成y关于f水平的箱线图

<pre>
y = c(1600, 1610, 1650, 1680, 1700, 1700, 1780, 1500, 1640, 
      1400, 1700, 1750, 1640, 1550, 1600, 1620, 1640, 1600,
      1740, 1800, 1510, 1520, 1530, 1570, 1640, 1600)
f = factor(c(rep(1,7),rep(2,5),rep(3,8),rep(4,6)))
plot(f,y)
</pre>

plot(df)

plot(~expr)

plot(y~expr)

其中df是数据框，y是任意一个对象，expr是对象名称的表达式

<pre>
df = data.frame(
    Age = c(13, 13, 14, 12, 12, 15, 11, 15, 14, 14, 14,
            15, 12, 13, 12, 16, 12, 11, 15), 
    Height = c(56.5, 65.3, 64.3, 56.3, 59.8, 66.5, 51.3, 
               62.5, 62.8, 69.0, 63.5, 67.0, 57.3, 62.5, 
               59.0, 72.0, 64.8, 57.5, 66.5), 
    Weight = c(84.0, 98.0, 90.0, 77.0, 84.5,112.0, 50.5, 
              112.5,102.5,112.5,102.5,133.0, 83.0, 84.0,
               99.5,150.0,128.0, 85.0,112.0))
plot(df)
plot(~df$Age+df$Height)
plot(df$Weight~df$Age)
</pre>

- **pairs()**

<pre>
#pairs(x, labels, panel = points, ...,
#      lower.panel = panel, upper.panel = panel,
#      diag.panel = NULL, text.panel = textPanel,
#      label.pos = 0.5 + has.diag/3, line.main = 3,
#      cex.labels = NULL, font.labels = 1,
#      row1attop = TRUE, gap = 1, log = "")
#对矩阵或者数据框绘制各列的散点图
pairs(df)
</pre>

- **coplot()**

<pre>
#coplot(formula, data, given.values, panel = points, rows, columns,
#       show.given = TRUE, col = par("fg"), pch = par("pch"),
#       bar.bg = c(num = gray(0.8), fac = gray(0.95)),
#       xlab = c(x.name, paste("Given :", a.name)),
#       ylab = c(y.name, paste("Given :", b.name)),
#       subscripts = FALSE,
#       axlabels = function(f) abbreviate(levels(f)),
#       number = 6, overlap = 0.5, xlim, ylim, ...)
#绘制给定条件下的散点图
#coplot(a~b|c)
#等长变量，c为向量或因子
coplot(df$Weight~df$Height|df$Age)
coplot(lat ~ long | depth, data = quakes)
given.depth <- co.intervals(quakes$depth, number = 4, overlap = .1)
coplot(lat ~ long | depth, data = quakes, given.v = given.depth, rows = 1)
</pre>

- **dotchart()**

<pre>
#dotchart(x, labels = NULL, groups = NULL, gdata = NULL,
#         cex = par("cex"), pch = 21, gpch = 21, bg = par("bg"),
#         color = par("fg"), gcolor = par("fg"), lcolor = "gray",
#         xlim = range(x[is.finite(x)]),
#         main = NULL, xlab = NULL, ylab = NULL, ...)
dotchart(VADeaths, main = "Death Rates in Virginia - 1940")
dotchart(t(VADeaths), xlim = c(0,100), main = "Death Rates in Virginia - 1940")
</pre>

- **contour()**

<pre>
#contour(x = seq(0, 1, length.out = nrow(z)),
#        y = seq(0, 1, length.out = ncol(z)),
#        z,
#        nlevels = 10, levels = pretty(zlim, nlevels),
#        labels = NULL,
#        xlim = range(x, finite = TRUE),
#        ylim = range(y, finite = TRUE),
#        zlim = range(z, finite = TRUE),
#        labcex = 0.6, drawlabels = TRUE, method = "flattest",
#        vfont, axes = TRUE, frame.plot = axes,
#        col = par("fg"), lty = par("lty"), lwd = par("lwd"),
#        add = FALSE, ...)
#绘制三维图形的等值线
x <- y <- seq(-4*pi, 4*pi, len = 27)
r <- sqrt(outer(x^2, y^2, "+"))
opar <- par(mfrow = c(2, 2), mar = rep(0, 4))
for(f in pi^(0:3))
  contour(cos(r^2)*exp(-r/f), drawlabels = FALSE, axes = FALSE, frame = TRUE)
</pre>

- **persp()**

<pre>
#persp(x = seq(0, 1, length.out = nrow(z)),
#      y = seq(0, 1, length.out = ncol(z)),
#      z, xlim = range(x), ylim = range(y),
#      zlim = range(z, na.rm = TRUE),
#      xlab = NULL, ylab = NULL, zlab = NULL,
#      main = NULL, sub = NULL,
#      theta = 0, phi = 15, r = sqrt(3), d = 1,
#      scale = TRUE, expand = 1,
#      col = "white", border = NULL, ltheta = -135, lphi = 0,
#      shade = NA, box = TRUE, axes = TRUE, nticks = 5,
#      ticktype = "simple", ...)
x <- seq(-10, 10, length= 30)
y <- x
f <- function(x, y) { r <- sqrt(x^2+y^2); 10 * sin(r)/r }
z <- outer(x, y, f)
z[is.na(z)] <- 1
persp(x, y, z, theta = 30, phi = 30, expand = 0.5, col = "lightblue",
      ltheta = 120, shade = 0.75, ticktype = "detailed",
      xlab = "X", ylab = "Y", zlab = "Sinc( r )")
x = y = seq(-2*pi, 2*pi, pi/15)
f = function(x, y) sin(x)*sin(y)
z = outer(x, y, f)
contour(x,y,z,col="blue")
persp(x, y, z, theta=30, phi=30, expand=0.7, col="lightblue")
</pre>

- **参数命令**

1. add=TRUE 表示所绘图形在原图上加图，默认值为FALSE。
2. axes=FALSE 表示所绘图形没有坐标轴，默认值为TRUE。
3. log="x" 表示x轴的数据取对数，log="y" 表示y轴的数据取对数，log="xy" 表示x轴与y轴的数据同时取对数。
4. type="p" 表示绘制散点图(默认值)，type="l" 表示绘制实线，type="o" 表示实线通过所有的点，type="h" 表示绘制出点到x轴的竖线，type="s" 表示绘制阶梯形曲线，type="n" 表示不绘制任何点或曲线。
5. xlab="str" 表示x轴标签为str， ylab="str" 表示y轴标签为str， main="str" 表示图的标签为str， sub="str" 表示子图的标签为str。 

<pre>
#points(x, y = NULL, type = "p", ...)
#在图上加点
#lines(x, y = NULL, type = "l", ...)
#在图上加线
#text(x, y = NULL, labels = seq_along(x), adj = NULL,
#     pos = NULL, offset = 0.5, vfont = NULL,
#     cex = 1, col = NULL, font = NULL, ...)
#在图上加文字
plot(-1:1, -1:1, type = "n", xlab = "Re", ylab = "Im")
K <- 16; text(exp(1i * 2 * pi * (1:K) / K), col = 2)
#abline(a = NULL, b = NULL, h = NULL, v = NULL, reg = NULL,
#       coef = NULL, untf = FALSE, ...)
#在图上加直线
#abline(a,b)表示画一条y=a+bx的直线
#abline(h=y)表示画一条水平直线
#abline(v=x)表示画一条垂直直线
#abline(df)对数据框或矩阵使用表示画一条拟合直线
#polygon(x, y = NULL, density = NULL, angle = 45,
#        border = NULL, col = NA, lty = par("lty"),
#        ..., fillOddEven = FALSE)
x <- c(1:9, 8:1)
y <- c(1, 2*(5:3), 2, -1, 17, 9, 8, 2:9)
op <- par(mfcol = c(3, 1))
for(xpd in c(FALSE, TRUE, NA)) {
  plot(1:10, main = paste("xpd =", xpd))
  box("figure", col = "pink", lwd = 3)
  polygon(x, y, xpd = xpd, col = "orange", lty = 2, lwd = 2, border = "red")
}
#title(main = NULL, sub = NULL, xlab = NULL, ylab = NULL,
#      line = NA, outer = FALSE, ...)
#在图上加说明文字
#axis(side, at = NULL, labels = TRUE, tick = TRUE, line = NA,
#     pos = NA, outer = FALSE, font = NA, lty = "solid",
#     lwd = 1, lwd.ticks = lwd, col = NULL, col.ticks = NULL,
#     hadj = NA, padj = NA, ...)
#在坐标轴上加标记、说明或其他内容(side取值为c(1:4)表示下左上右)
</pre>