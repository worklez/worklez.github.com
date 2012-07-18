---
layout: post
category : blog
title: JavaScript efficiency
tags : [dev, javascript, wtf]
---
{% include JB/setup %}

Сразу оговорюсь: я далеко не специалист в JavaScript.

Как эффективнее (по времени выполнения) склеить два массива в JavaScript?
Мне приходят в голову два варианта:

{% highlight js %}
foo = foo.concat(bar)
{% endhighlight %}

и

{% highlight js %}
bar.forEach(function(x) { foo.push(x) })
{% endhighlight %}

Оба варианта, прямо скажем, не идеальны.
Идельным вариантом, на мой взгляд, было бы что-то вроде

{% highlight js %}
foo.append(bar)
{% endhighlight %}

в результате которого `foo` бы сначала увеличился до нужного размера (или больше)
а затем отработало быстрое копирование. Что-то подобное возможно?

Miniquiz:
В MapReduce MongoDB используется JavaScript-движок SpiderMonkey. Какой вариант, первый или второй, будет быстрее сливать мильёны объектов в reduce-функции?

(спойлер: <span style="color: #fff">второй. до двух раз быстрее</span>)
