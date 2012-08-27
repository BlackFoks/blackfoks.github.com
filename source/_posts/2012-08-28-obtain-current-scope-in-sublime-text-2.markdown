---
layout: post
title: "Obtain current scope in Sublime Text 2"
date: 2012-08-28 00:56
comments: true
categories:
---

In order to obtain current scope under the cursor in Sublime Test 2 just run
this command in the sublime console:

{% codeblock lang:sh %}
print (view.syntax_name(view.sel()[0].b))
{% endcodeblock %}
