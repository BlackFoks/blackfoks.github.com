---
layout: post
title: "Настройка Capistrano вместе с RVM, Bundler и Fusion Passanger под AWS"
date: 2012-01-17 17:03
comments: true
categories:
---

Для начала установим Capistrano:

{% codeblock Gemfile lang:ruby %}
gem 'capistrano'
{% endcodeblock %}

{% codeblock lang:sh %}
$ bundle install
{% endcodeblock %}

Теперь инициализируйте Capistrano в рабочей папке проекта:

{% codeblock lang:sh %}
$ cd my_project
$ capify .
{% endcodeblock %}

Сначала я думал поэтапно все расписать, но потом решил, что это излишне и гораздо
проще, удобней и понятней будет привести конечный код моего Capfile с моими
комментариями на русском.

{% gist 3487103 %}

Ну вот вроде и все.

Ссылки
------

- [Rails 3, RVM, Passenger and Capistrano](http://kris.me.uk/2010/08/05/rails3-rvm-passenger-capistrano.html)
- [Rails 3.1 / Rack App Hosting with RVM, Passenger 3, Capistrano, Git and Apache](http://kris.me.uk/2011/10/28/rails-rvm-passenger-capistrano-git-apache.html)
- [Using RVM rubies with Capistrano](http://beginrescueend.com/integration/capistrano/)
- [Craic Computing Tech Tips](http://craiccomputing.blogspot.com/2008/08/ec2-ssh-and-capistrano.html)
- [Capistrano recipe for passenger standalone](https://gist.github.com/959444)
- [Deploy with Capistrano](http://help.github.com/deploy-with-capistrano/)
- [Пример файла deploy.rb от пользователя с хабра](http://pastebin.com/WnHJpbEq)
