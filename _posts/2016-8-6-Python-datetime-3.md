---
layout: post
title:  "Python时间处理(3)"
author: E君
date:   2016-8-5
categories: Python
tags: 学习笔记
---

### datetime对象 ###

datetime对象是一个包含所有date对象和time对象信息的简单对象。

datatime对象能够向date对象那样在公历上扩展，也能像time对象那样提供每天精确的3600*24秒。

- 类：
<pre>
datetime.datetime(year, month, day[, hour[, minute[, second[, microsecond[, tzinfo]]]]])
# year，month，day三个参数是必须的
# 所有参数必须为整型或长整型，而且在下面的范围内
# MINYEAR <= year <= MAXYEAR
# MINYEAR = 1， MAXYEAR = 9999
# 1 <= month <= 12
# 1 <= day <= 给定年月后当月的最大天数
# 0 <= hour <24
# 0 <= minute < 60
# 0 <= second < 60
# 0 <= microsecond < 1000000
# 如果参数超出了范围，将抛出ValueError
</pre>
- 类方法：
<pre>
datetime.datetime.today()
# 返回当前日期和时间，等同于datetime.datetime.fromtimestamp(time.time())
datetime.datetime.now([tz])
# 返回当前日期和时间
# 如果参数tz为None或者未指定，就与today()一样
# tz必须为tzinfo的子类，并且当前时间将转换成tz的时区
datetime.datetime.utcnow()
# 返回当前UTC时间，相当于tzinfo为None
datetime.datetime.fromtimestamp(timestamp[, tz])
# 根据给定的POSIX时间戳返回日期和时间，例如time.time()生成的时间戳
# 如果指定了可选参数tz，那么当前时间将转换成tz的时区
# 如果时间戳的范围超过C平台的localtime()函数返回值，可能会引起ValueError
datetime.datetime.utcfromtimestamp(timestamp)
# 根据给定的POSIX时间戳返回日期和时间，相当于tzinfo为None
datetime.datetime.fromordinal(ordinal)
# 根据距公元1年1月1日的天数来判定日期
# datetime.date(1,1,1).toordinal() == 1
# 对于任意一天，datetime.date.fromordinal(d.toordinal()) == d
# hour,minute,second,microsecond都为0，并且tzinfo为None
datetime.datetime.combine(date, time)
# 返回一个新的datetime对象，其日期等于参数中的date对象，时间等于参数中的time对象
# 对于任何datetime对象，d == datetime.combine(d.date(), d.timetz())
# 如果date是一个datetime对象，它的时间部分和tzinfo都被忽略
datetime.datetime.strptime(date_string, format)
# 根据给定的format，对date_string进行解析并返回datetime对象
# This is equivalent to datetime(*(time.strptime(date_string, format)[0:6]))
# 如果解析失败，或者返回的值不能构成一个时间tuple，则会抛出ValueError
</pre>

- 类属性

<pre>
datetime.date.min
# 最小值 datetime.datetime(MINYEAR, 1, 1, tzinfo=None)
datetime.date.max
# 最大值 datetime.datetime(MAXYEAR, 12, 31, 23, 59, 59, 999999, tzinfo=None)
datetime.date.resolution
# 精度 datetime.timedelta(microseconds =1)
</pre>

- 对象属性：

<pre>
year
# MINYEAR <= year <= MAXYEAR 
month
# 1 <= month <= 12
day
# 1 <= day <= 给定年月后当月的最大天数
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

- 支持的运算符：

<pre>
datetime2 = datetime1 + timedelta
datetime2 = datetime1 - timedelta
timedelta = datetime1 - datetime2
datetime1 < datetime2
</pre>

- 对象方法：

<pre>
date()
# 返回具有相同year,month,day的date对象
time()
# 返回具有相同hour,minute,second,microsecond的time对象，tzinfo为None
timetz()
# 返回具有相同hour,minute,second,microsecond,tzinfo的time对象
replace([year[, month[, day[, hour[, minute[, second[, microsecond[, tzinfo]]]]]]]])
# 返回一个相同的值，除了给定参数不一样
# ^_^! 这个鬼有什么用，不能新建一个datetime对象么~
# 注意能够指定tzinfo=None来根据任何时间创造一个一般时间
astimezone(tz)
# 返回一个datetime对象并具有新的tz属性，调整time和date数据以保证返回值的结果和它本身具有相同的UTC时间，但tz为当地时间
utcoffset()
# 如果tzinfo为None，返回None，否则返回self.tzinfo.utcoffset(self)
dst()
# 如果tzinfo为None，返回None，否则返回self.tzinfo.dst(self)
tzname()
# 如果tzinfo为None，返回None，否则返回self.tzinfo.tzname(self)
timetuple()
# 返回一个time.struct_time，其中hours，minutes，seconds均为0，DST标签为-1。
# d.timetuple()等同于time.struct_time((d.year, d.month, d.day, d.hour, d.minute, d.second, d.weekday(), yday, dst))
# yday = d.toordinal() - date(d.year, 1, 1).toordinal() + 1 是这天距这年1月1日的天数
# tm_isdst标签根据dst()方法来设定：如果tzinfo为None或dst()返回None，tm_isdst被设为-1；如果dst()返回一个非零值，tm_isdst被设为1；否则tm_isdst被设为0
utctimetuple()
# 如果datetime对象是naive的，与d.timetuple()等同，并且无论d.dst()返回什么值，tm_isdst均设为0
# 如果date对象是aware的，d将被减去d.utcoffset()转换成标准UTC时间，并且返回标准时间的time.struct_time。tm_isdst被强制设为0
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
isoformat([sep])
# 返回ISO8601表示的格式，YYYY-MM-DDTHH:MM:SS.mmmmmm
# 如果utcoffset不返回None，在最后将加上与UTC的偏差：YYYY-MM-DDTHH:MM:SS.mmmmmm+HH:MM
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
