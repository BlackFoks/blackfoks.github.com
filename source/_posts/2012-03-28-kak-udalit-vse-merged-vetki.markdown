---
layout: post
title: "Как удалить все merged ветки"
date: 2012-03-28 18:58
comments: true
categories:
---

{% codeblock lang:sh %}
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
{% endcodeblock %}
