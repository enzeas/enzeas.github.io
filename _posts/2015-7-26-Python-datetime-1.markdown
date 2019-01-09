---
layout: post
title:  "Python时间处理(1)"
categories: Python
tags: "学习笔记" 
---

### Python datetime ###

- 常量：
<pre>
datetime.MINYEAR
# date/datetime对象中允许的最小时间，1
datetime.MAXYEAR
# date/datetime对象中允许的最大时间，9999
</pre>
- 类：
<pre>
datetime.date
# 日期，属性为：year，month，day
datetime.time
# 时间，属性为：hour，minute，second，microsecond，tzinfo
datetime.datetime
# 日期时间，属性为：year，month，day，hour，minute，second，microsecond，tzinfo
datetime.timedelta
# 时间差，两个date，time，datetime之间的差异，精度为微秒
datetime.tzinfo
# 时区信息的抽象基类，在time和datetime中提供自定义的注解
</pre>

### timedelta ###

- 定义
<pre>
class datetime.timedelta([days[, seconds[, microseconds[, milliseconds[, minutes[, hours[, weeks]]]]]]])
</pre>
所有参数都是可选的，并且默认为0。

参数可以是整数，长整数或者浮点数，并且可正可负。

实际上内部仅存储了days，seconds，microseconds，其他的参数都被转换成这些单位：

1 millisecond -> 1000 microseconds

1 minute -> 60 seconds

1 hour -> 3600 seconds

1 week -> 7 days

为了保证描述的唯一性，会将days，seconds，microseconds标准化到一定范围：

0 <= microseconds < 1000000

0 <= seconds < 3600*24 (一天的秒数)

-999999999 <= days <= 999999999

如果经过标准化后days的值超出了可表达的范围，将会抛出OverflowError异常。

注意负值的标准化可能会看上去有些吃惊：

<pre>
>>> from datetime import timedelta
>>> d = timedelta(microseconds=-1)
>>> (d.days, d.seconds, d.microseconds)
(-1, 86399, 999999)
</pre>


**timedelta类的主要属性有：**

- 类属性：

<pre>
datetime.timedelta.min
# 最小值 timedelta(-999999999)
datetime.timedelta.max
# 最大值 datetime.timedelta(999999999, 86399, 999999)
datetime.timedelta.resolution
# 精度，datetime.timedelta(0, 0, 1)
</pre>

- 对象属性：

<pre>
days
# -999999999 <= days <= 999999999 
seconds
# 0 <= seconds <= 86399
microseconds
# 0 <= microseconds <= 999999
</pre>

- 支持的运算符：

<pre>
t1 = t2 + t3
t1 = t2 - t3
t1 = t2 * i or t1 = i * t2
t1 = t2 // i
+t1
-t1
abs(t)
str(t)
repr(t)	
</pre>

- 对象方法：

<pre>
total_seconds()
# 返回持续时间的秒数，等于(td.microseconds + (td.seconds + td.days * 24 * 3600) * 10**6) / 10**6
</pre>
