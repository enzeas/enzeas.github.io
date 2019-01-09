---
layout: post
title:  "Python时间处理(4)"
categories: Python
tags: "学习笔记" 
---

### time对象 ###

time对象包含一天的时间，独立代表任何一天，并能通过tzinfo对象进行调整。

- 类：

<pre>
datetime.time([, hour[, minute[, second[, microsecond[, tzinfo]]]]])
# 所有参数都是可选的。tzinfo必须为None或者tzinfo子类对象。
# 其余参数必须为整型或长整型，而且在下面的范围内
# 0 <= hour <24
# 0 <= minute < 60
# 0 <= second < 60
# 0 <= microsecond < 1000000
# 如果参数超出了范围，将抛出ValueError
</pre>

- 类属性：

<pre>
datetime.time.min
# 最小值，datetime.time(0, 0, 0, 0)
datetime.time.max
# 最大值，datetime.time(23, 59, 59, 999999)
datetime.time.resolution
# 精度，datetime.timedelta(microseconds=1)
</pre>

- 对象属性(只读)：

<pre>
hour
# range(24)
minute
# range(60)
second
# range(60)
microsecond
# range(1000000)
tzinfo
# 通过datetime构造函数传入的tzinfo，或者为None
</pre>

- 支持的运算：

1. 比较，如果时区不同，则调整为UTC标准时间再进行比较。
2. 哈希，用作字典的建。
3. 有效率地储存(pickle)。
<!-->4. 在条件上下文中，当且仅当time被转化成分钟并减去utcoffset()后值为非零时，为True。<-

- 对象方法：

<pre>
replace([hour[, minute[, second[, microsecond[, tzinfo]]]]])
# 返回一个相同的值，除了给定参数不一样
# ^_^! 这个鬼有什么用，不能新建一个datetime对象么~
# 注意能够指定tzinfo=None来根据任何时间创造一个一般时间
isoformat()
# 返回ISO8601表示的格式，HH:MM:SS.mmmmmm
# 如果utcoffset不返回None，在最后将加上与UTC的偏差：YYYY-MM-DDTHH:MM:SS.mmmmmm+HH:MM
__str__()
# 等同于t.isoformat()
strftime(format)
# 通过一个显性的格式字符串，返回一个字符串代表当前日期。
# 时分秒将为0值。
__format__(format)
# 等同于time.strftime()
utcoffset()
# 如果tzinfo为None，返回None，否则返回self.tzinfo.utcoffset(None)
dst()
# 如果tzinfo为None，返回None，否则返回self.tzinfo.dst(None)
tzname()
# 如果tzinfo为None，返回None，否则返回self.tzinfo.tzname(None)
</pre>
