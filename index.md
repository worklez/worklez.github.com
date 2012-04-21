---
layout: page
title: Hello World!
---
{% include JB/setup %}

## Social
![](http://twitter.com/favicon.ico) [Twitter](https://twitter.com/worklez)

![](http://linkedin.com/favicon.ico) [LinkedIn](http://linkedin.com/in/worklez)

![](http://github.com/favicon.ico) [GitHub](https://github.com/worklez)

![](http://moikrug.ru/favicon.ico) [Мой Круг](http://worklez.moikrug.ru)

![](http://last.fm/favicon.ico) [last.fm](http://last.fm/user/worklez)

![](http://vk.com/favicon.ico) [vk.com](http://vk.com/id727886)


## Записи

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
