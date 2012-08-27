---
layout: post
title: "Настраиваем PostgreSQL в Ubuntu и Mac OS X для RoR"
date: 2012-01-16 11:46
comments: true
categories:
---

Начал экспериментировать с AWS и для блога решил выбрать PostgreSQL. В этом
посте я опишу, как настроить эту БД в ubuntu на aws и на маке.

Установка и настройка в Ubuntu 10.04
------------------------------------

Мы будем ставить последнюю на дату написания поста версию 9.1.2. Так как для
10.04 в стандартных репах этой версии нету, то надо подключить ppa.

{% codeblock lang:sh %}
$ sudo add-apt-repository ppa:pitti/postgresql
$ sudo apt-get update
{% endcodeblock %}

Теперь можно начать установку postgresql. Также установим библиотеку libpq-dev,
которая требуется для гема `pg`.

{% codeblock lang:sh %}
$ sudo apt-get install postgresql-9.1 libpq-dev
{% endcodeblock %}

После установки давайте перезапустим сервер с поддержкой юникода:

{% codeblock lang:sh %}
$ sudo -u postgres pg_dropcluster --stop 9.1 main
$ sudo -u postgres pg_createcluster --start -e UTF-8 9.1 main
{% endcodeblock %}

По умолчанию при установке был создан новый пользователь postgres, давайте
зайдем в оболочку postgresql под его именем:

{% codeblock lang:sh %}
$ sudo -u postgres psql postgres
{% endcodeblock %}

Далее давайте зададим пароль этому пользователю:

{% codeblock lang:sh %}
postgres=# \password postgres
{% endcodeblock %}

Пароль не будет отображаться при вводе, вас попросят ввести пароль два раза.
Теперь вы можете указывать этого пользователя и его пароль в `database.yaml`.

Установка и настройка в Mac OS X 10.7.2
---------------------------------------

Установку в mac os будет производить с использованием Homebrew.

{% codeblock lang:sh %}
$ brew update
$ brew install postgresql
{% endcodeblock %}

После чего, следуя инструкциям, инициализируем БД и добавим postgresql в
автозагрузку.

{% codeblock lang:sh %}
$ initdb /usr/local/var/postgres
$ mkdir -p ~/Library/LaunchAgents
$ cp /usr/local/Cellar/postgresql/9.1.2/org.postgresql.postgres.plist ~/Library/LaunchAgents/
$ launchctl load -w ~/Library/LaunchAgents/org.postgresql.postgres.plist
{% endcodeblock %}

Формула Homebrew не создает пользователя, аналогичного пользователю postgres в
ubuntu. По умолчанию, аутентификация в БД происходит под тем же пользователем,
под которым была выполнена команда `brew install`. Пароль же (по крайней мере
в database.yaml) можно вообще не указывать.

Все, теперь вы можете установить гем `pg` как обычно и указать нужные параметры
в database.yaml.

Настройка pg и рельс
--------------------

Добавьте гем `pg` в ваш Gemfile и выполните `bundle install` как обычно. Теперь
в укажите в database.yml нужные параметры. Я использую такие:

{% codeblock database.yml %}
development:
  adapter: postgresql
  encoding: unicode
  pool: 5
  username: {username}
  password:
  host: localhost
  port: 5432
  database: {project_name}_dev

production:
  adapter: postgresql
  encoding: unicode
  pool: 5
  username: postgres
  password: {password}
  host: localhost
  port: 5432
  min_messages: warning
  database: {project_name}
{% endcodeblock %}

Возможно, что эти параметры не совсем оптимальны, т.ч. будьте осторожны :)

Ссылки
------

Пост написан на основе этих материалов:

- [Установка Postgresql 9, pgAdmin III в Ubuntu 10.04](http://lug.nsk.ru/lugnskru/2010/10/ustanovka-postgresql-9-pgadmin-iii-v-ubuntu-10-04.html)
- [Installing PostgreSQL 9.0 on Ubuntu 10.04](http://www.dctrwatson.com/2010/09/installing-postgresql-9-0-on-ubuntu-10-04/)
- [Install PostgreSQL 9 on OS X](http://russbrooks.com/2010/11/25/install-postgresql-9-on-os-x)

Также привожу ссылку на указанный в посте ppa:

- [PostgreSQL backports for stable Ubuntu releases](https://launchpad.net/~pitti/+archive/postgresql)

