---
layout: post
category : blog
title: Python и утечки
tags : [dev, python, leaks]
---
{% include JB/setup %}

Утечки памяти при работе с Python можно разделить на два типа:
1. &nbsp;питоновые объекты, более не используемые, но по-прежнему сидящие в памяти,
2. &nbsp;области памяти, выделенные по запросу нативного кода и им не освобождённые.

Первый тип утечек легко определяется - достаточно воспользоваться сборщиком мусора с помощью модуля `gc` и
посмотреть статистику. Объекты могут утекать, например, из-за наличия кольцевых ссылок,
как это [происходит](http://http://bugs.python.org/issue1208304) при каждом вызове `urllib2.urlopen()`.
Баг, кстати, ещё жив в Python 2.6/2.7. Из-за этого (а также из-за
[разнообразия](http://www.voidspace.org.uk/python/articles/urllib2.shtml#handling-exceptions) вылетаемых исключений,
но это совсем другая история)
в демонах надёжнее вообще не использовать `urllib2`.

Второй тип гораздо вреднее. Встроенными питоновыми средствами таких утечек не обнаружить,
помочь может разве что прогон через `valgrind`.

Утечку памяти через нативный код можно найти в python-обёртке библиотеки `libxml2`.
У объекта, представляющего XML-документ, нужно не забывать
позвать метод `freeDoc()` (разработчики не заморочились реализацией `__del__`), но это только полбеды.

Miniquiz: что не так в этом коде?

{% highlight python %}
doc = libxml2.parseFile("test.xml")
res = doc.xpathEval("/foo//bar")
doc.freeDoc()
{% endhighlight %}

Вроде бы всё в порядке.

Посмотрим на `xpathEval()` подробнее:

{% highlight python %}
def xpathEval(self, expr):
  doc = self.doc
  if doc == None:
    return None
  ctxt = doc.xpathNewContext()
  ctxt.setContextNode(self)
  res = ctxt.xpathEval(expr)
  ctxt.xpathFreeContext()
  return res
{% endhighlight %}

Теперь рассмотрим `ctxt.xpathEval()`, который вызывается из предыдущего метода:

{% highlight python %}
def xpathEval(self, str):
  """Evaluate the XPath Location Path in the given context. """
  ret = libxml2mod.xmlXPathEval(str, self._o)
  if ret is None:
    raise xpathError('xmlXPathEval() failed')
  return xpathObjectRet(ret)
{% endhighlight %}

Вот оно! Как только по XPath-запросу ничего не нашлось, что может вполне соответствовать бизнес-логике,
выкидывается весёлое исключение и контекст `ctxt` не освобождается (не вызывается `ctxt.xpathFreeContext()`).

Раз с такими утечками трудно бороться, нужно их избегать.

Самый простой способ избежать утечек через нативный код, которому я решил следовать, - не использовать нативный код. :)
Часто можно воспользоваться аналогичной библиотекой, реализованной полностью на Python.
А если уж возникнет проблема с производительностью и нужно будет её решать - тогда можно подробно изучить
нативный инструмент и со всей осторожностью его применить.
