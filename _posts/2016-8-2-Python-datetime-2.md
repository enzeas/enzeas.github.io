---
layout: post
title:  "Python时间处理(2)"
author: E君
date:   2016-8-2
categories: Python
tags: 学习笔记
---

### date对象 ###

- 类：
<pre>
datetime.date(year, month, day)
# 所有的参数都是必须的
# 参数必须为整型或长整型，而且在下面的范围内
# MINYEAR <= year <= MAXYEAR
# MINYEAR = 1， MAXYEAR = 9999
# 1 <= month <= 12
# 1 <= day <= 给定年月后当月的最大天数
# 如果参数超出了范围，将抛出ValueError
</pre>
- 类方法：
<pre>
datetime.date.today()
# 返回当前日期，等同于datetime.date.fromtimestamp(time.time())
datetime.date.fromtimestamp(timestamp)
# 根据给定的时间戳返回日期
# 如果时间戳的范围超过C平台的localtime()函数返回值，可能会引起ValueError
datetime.date.fromordinal(ordinal)
# 根据距公元1年1月1日的天数来判定日期
# datetime.date(1,1,1).toordinal() == 1
# 对于任意一天，datetime.date.fromordinal(d.toordinal()) == d
</pre>

- 类属性

<pre>
datetime.date.min
# 最小值 datetime.date(MINYEAR, 1, 1)
datetime.date.max
# 最大值 datetime.date(MAXYEAR, 12, 31)
datetime.date.resolution
# 精度 datetime.timedelta(days=1)
</pre>

- 对象属性：

<pre>
year
# MINYEAR <= year <= MAXYEAR 
month
# 1 <= month <= 12
day
# 1 <= day <= 给定年月后当月的最大天数
</pre>

- 支持的运算符：

<pre>
date2 = date1 + timedelta
date2 = date1 - timedelta
timedelta = date1 - date2
date1 < date2
</pre>

- 对象方法：

<pre>
replace(year, month, day)
# 返回一个相同的值，除了给定参数不一样
# ^_^! 这个鬼有什么用，不能新建一个date对象么~
timetuple()
# 返回一个time.struct_time，其中hours，minutes，seconds均为0，DST标签为-1。
# d.timetuple()等同于time.struct_time((d.year, d.month, d.day, 0, 0, 0, d.weekday(), yday, -1))
# yday = d.toordinal() - date(d.year, 1, 1).toordinal() + 1 是这天距这年1月1日的天数
toordinal()
# 返回这天距公元1年1月1日的天数
weekday()
# 返回一个整数代表星期几，其中星期一返回0，星期天返回6
isoweekday()
# 返回一个整数代表星期几，其中星期一返回1，星期天返回7
isocalendar()
# 返回一个三元元组(ISO year, ISO week number, ISO weekday)
# ISO年由52或53个星期组成，每周由周一开始，周六结束。
# ISO年的第一周是公历年中包含第一个周四的周
# 例如，2004年的第一天是周四，因此ISO年2004的第一周由2003年12月29日开始，2004年1月4日结束。
# 因此，date(2003, 12, 29).isocalendar() == (2004, 1, 1)
# date(2004, 1, 4).isocalendar() == (2004, 1, 7)
isoformat()
# 返回ISO8601表示的格式，YYYY-MM-DD
__str__()
# 等同于d.isoformat()
ctime()
# 返回一个字符串代表当前日期，例如date(2002, 12, 4).ctime() == 'Wed Dec 4 00:00:00 2002'
# 相当于time.ctime(time.mktime(d.timetuple()))
strftime(format)
# 通过一个显性的格式字符串，返回一个字符串代表当前日期。
# 时分秒将为0值。
__format__(format)
# 等同于date.strftime()
</pre>
