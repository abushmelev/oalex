---
published: true
title: Когда нет места в каталоге boot 
excerpt: Нет свободного места на диске, содержащим каталог boot. Как почистить диск от старых ядер Linux
icon: fa fa-linux
modified: 2015-05-25
layout: post
categories: [articles, linux]
tags: [linux, ubuntu]
comments: true
image:
  feature:
share: true
---       

Иногда такое случается. Пришло обновление, а установится не может. Ну нет больше свободного места на диске,
содержащим каталог boot. У меня такое происходит регулярно на рабочем компе. Выдали уже по-жмотски отформатированныи и залитым. Что делать?
<!-- more -->
Решение-то простое, взять и удалить старые неиспользованные ядра. Но как удалить только ненужные?

Вот так можно посмотреть все установленные ядра:

`dpkg -l linux-image-\* | grep ^ii`

Последовательность команд, которая покажет список всех ядер и их описаний за исключением текущего "запущеного ядра"

{% highlight PowerShell linenos %}
kernelver=$(uname -r | sed -r 's/-[a-z]+//')
dpkg -l linux-{image,headers}-"[0-9]*" | awk '/ii/{print $2}' | grep -ve $kernelver
{% endhighlight %}
 
Важно, в перечень не входит именно работающее ядро, а не последнее!

И, наконец, долгожданная команда, которая прочистит все что не работает:

{% highlight PowerShell linenos %}
sudo apt-get purge $(dpkg -l linux-{image,headers}-"[0-9]*" | awk '/ii/{print $2}' | grep -ve "$(uname -r | sed -r 's/-[a-z]+//')")
{% endhighlight %}
 
Отработка этой команды выглядит страшно. В цикле удаляются неиспользующиеся ядра, при этом, после удаления каждого ядра выводится список оставшихся, в том числе и работающего. С замиранием сердца ждешь, когда удалится все, но нет, удалилось не все! Можно апдейтится. Место есть.

Проверено на ubuntu 14.04

Записано по мотивам ответа на [http://askubuntu.com/questions/89710/how-do-i-free-up-more-space-in-boot](http://askubuntu.com/questions/89710/how-do-i-free-up-more-space-in-boot){:target="_blank"}
