---
layout: post
title:  "Python时间处理(5)"
author: E君
date:   2016-8-18
categories: Python
tags: 学习笔记
---

### tzinfo对象 ###

tzinfo是一个抽象基类，意味着这个类不能被直接实例化。你需要派生出一个具体的子类，并且至少将你在datetime方法中使用到的标准tzinfo方法进行补充实现。datetime模块并不提供任何tzinfo的具体子类。

一个tzinfo的实例能够传递给datetime和time对象构造函数作为参数。time对象能够像本地时间一样查看对象属性，tzinfo对象支持显示从本地时间到UTC时间的偏移值、时区的名字、DST偏移值、以及一切传递给tzinfo的date或time对象。

一个tzinfo子类必须要有一个无参数的__init__()方法才能被pickle存储，否则可能不能完整取出。这是一个技术需求，可能会在未来修复。

tzinfo的具体子类可能需要实现下面的方法。确切的方法根据datetime对象的使用而定，如果有疑问的话，就把它们都实现了把。

<pre>
tzinfo.utcoffset(self, dt)
# 返回本地时间距时间标准时间(Universal Time Coordinated, UTC)时间的偏移值，根据UTC时间的东部算起。如果本地时间在UTC时间的西部，返回值则为负。
# 注意这个函数意图返回距UTC时间的总偏移值；例如，如果一个tzinfo对象代表了市区和DST(夏令时)调整，utcoffset()应该返回它们的总和。
# 如果UTC偏移值是未知的，返回None。
# 此外，返回值必须是一个timedelta对象，必须包含在-1439到1439范围内（1440=24*60；偏移值的量级必须小于一天），代表整个分钟。
# 大多数的utcoffset()实现起来可能像这样：
return CONSTANT                 # fixed-offset class
return CONSTANT + self.dst(dt)  # daylight-aware class
# 如果utcoffset()并不返回None，那么dst()也不应该返回None。
# utcoffset()的默认实现会抛出一个异常NotImplementedError。

tzinfo.dst(self, dt)
# 返回夏令时(daylight saving time, DST)的偏移值，单位为分钟，或者当DST信息未知时返回None。
# 如果DST无效，返回timedelta(0)。
# 如果DST有效，返回一个timedelta对象作为偏移值(详见utcoffset)。
# 在合适的情况下，需要注意到夏令时偏移值已经被加到了utcoffset()返回的世界标准时间偏移值，因此除非需要单独获取世界标准时间的信息，不需要查阅dst()。
# 例如，datetime.timetuple()调用了它tzinfo属性的dst()方法来判断是否需要设置tm_isdst，tzinfo.fromutc()调用dst()来解释跨越时区时的夏令时改变。
# 一个tzinfo子类的tz实例在这种情况下应该一致实现标准时间和夏令时：
tz.utcoffset(dt) - tz.dst(dt)
# 必须对每个dt.tzinfo==tz的datetime实例dt都返回相同的结果。
# 对于健全的tzinfo子类，这个表达式产生了时区的“标准偏移量”，不依赖于日期或时间，而应该仅依赖地理位置。
# datetime.astimezone()的实现依赖于这些函数，但并不会察觉到违反这些条例；这应该由开发者去保障。
# 如果一个tzinfo子类未能确保实现这些，可以通过覆盖(override)tzinfo.fromutc()的默认实现来使astimezone()正确运作。
大多数dst()的实现起来可能像这样：
def dst(self, dt):
    # a fixed-offset class:  doesn't account for DST
    return timedelta(0)
# 或者
def dst(self, dt):
    # Code to set dston and dstoff to the time zone's DST
    # transition times based on the input dt.year, and expressed
    # in standard local time.  Then
    if dston <= dt.replace(tzinfo=None) < dstoff:
        return timedelta(hours=1)
    else:
        return timedelta(0)
# dst()的默认实现会抛出一个异常NotImplementedError。

tzinfo.tzname(self, dt)
# 返回与之对应的datetime对象dt的时区名，通常是字符串。
</pre>

