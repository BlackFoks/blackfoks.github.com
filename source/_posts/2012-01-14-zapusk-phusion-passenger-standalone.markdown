---
layout: post
title: "Запуск Phusion Passenger (Standalone)"
date: 2012-01-14 16:59
comments: true
categories:
---

Запуск passenger'а на 80 порту в продакшене:

{% codeblock lang:sh %}
$ rvmsudo passenger start -p 80 --user=ubuntu -e production
{% endcodeblock %}
