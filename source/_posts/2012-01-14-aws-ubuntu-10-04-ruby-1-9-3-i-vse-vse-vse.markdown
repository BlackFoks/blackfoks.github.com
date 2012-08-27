---
layout: post
title: "AWS, Ubuntu 10.04, Ruby 1.9.3 и все-все-все"
date: 2012-01-14 16:56
comments: true
categories:
---

Смотрим [список AMI для 10.04](http://uec-images.ubuntu.com/lucid/current/),
выбираем, какой больше нравится. Я выбрал 32-битку ebs в регионе us-east-1 (под
халявный микро-инстанс). Там же указана команда, которой можно создать инстанс с
соответствующим AMI. Для выбранного мной инстанса команда такая:

{% codeblock lang:sh %}
$ ec2-run-instances ami-f5d1069c --instance-type t1.micro --region us-east-1 --key ${EC2_KEYPAIR_US_EAST_1}
{% endcodeblock %}

Не забудьте указать используемый вами ключ.

Подождите, пока инстанс создастся (посмотреть можно в AWS Management Console),
после чего коннектимся к только что созданному инстансу:

{% codeblock lang:sh %}
$ ssh -i {ваш-ключ} ubuntu@ec2-xx-xx-xx-xx.compute-1.amazonaws.com
{% endcodeblock %}

Теперь приступаем к следующему этапу.

Установка RVM и Ruby
-------------------

Для начала давайте убедимся, что у нас вообще нету руби:

{% codeblock lang:sh %}
$ which ruby
$
{% endcodeblock %}

Точно, нету. Ставим curl, git и обычную ruby (требуется для уставки MRI через rvm):

{% codeblock lang:sh %}
$ sudo apt-get install curl git-core ruby
{% endcodeblock %}

Теперь ставим rvm ([подробные инструкции тут](http://rvm.beginrescueend.com/rvm/install/)):

{% codeblock lang:sh %}
$ bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer )
{% endcodeblock %}

После установки гляньте заметки об rvm:

{% codeblock lang:sh %}
$ rvm notes
{% endcodeblock %}

И обязательно

{% codeblock lang:sh %}
$ rvm requirements
{% endcodeblock %}

для того, чтобы посмотреть, что нужно установить для используемой ОС. В моем
случае мне предложили это:

{% codeblock lang:sh %}
$ sudo apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion
{% endcodeblock %}

Что я и сделал. Теперь надо добавить запись об rvm в .bash-profile:

{% codeblock lang:sh %}
$ echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile
{% endcodeblock %}

И перезагрузим профайл:

{% codeblock lang:sh %}
$ source .bash_profile
{% endcodeblock %}

Давайте проверим, все ли норм с rvm:

{% codeblock lang:sh %}
$ type rvm | head -1
rvm is a function
{% endcodeblock %}

Если все так, то все хорошо.

Теперь можно посмотреть список известных версий руби и поставить нужную:

{% codeblock lang:sh %}
$ rvm list known
$ rvm install 1.9.3-p0
{% endcodeblock %}

После чего делаем установленную версию руби версией по умолчанию:

{% codeblock lang:sh %}
$ rvm --default 1.9.3-p0
$ ruby -v
ruby 1.9.3p0 (2011-10-30 revision 33570) [i686-linux]
{% endcodeblock %}

Все, квест закончен :) Можно приступить к установке рельсов и всего остального.)

**UPD**. Также не забудьте добавить execjs и therubyracer в production секцию Gemfile.

{% codeblock Gemfile lang:ruby %}
gem 'execjs'
gem 'therubyracer'
{% endcodeblock %}
