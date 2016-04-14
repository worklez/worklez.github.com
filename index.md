---
layout: page
title: Hello World!
---
{% include JB/setup %}

## Social
<img src="https://twitter.com/favicon.ico" width="16"> [Twitter](https://twitter.com/worklez)

<img src="https://linkedin.com/favicon.ico" width="16"> [LinkedIn](http://linkedin.com/in/worklez)

<img src="https://github.com/favicon.ico" width="16"> [GitHub](https://github.com/worklez)

<img src="https://static-web.last.fm/static/images/favicon.ico" width="16"> [last.fm](http://last.fm/user/worklez)


## Записи

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
