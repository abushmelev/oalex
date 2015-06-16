---
published: true
title: Конвертация файлов в другую кодировку
excerpt: Потребовалось сконвертировать файлы из кодировки cp1251 в utf8
icon: fa fa-linux
date: 2015-06-16
modified: 2015-06-16
layout: post
categories: [articles]
tags: [ubuntu, iconv, vim]
comments: true
image:
    feature:
share: true
---

Потребовалось сконвертировать файлы, расположенные в подкаталогах, из кодировки cp1251 в utf8 в ubuntu 14.04.  Разве это проблема в linux? Оказывается да...

<!-- more -->

Долгие и продолжительные мучения с использованием `enconv` и `decode` закончились ничем. Команды срабатывали без ошибок и ... конвертации. Как было, так и осталось.

Дело сделала команда:

`find . -name "*.cpp" -exec iconv -f cp1251 -t utf8 -o {}.new {} \; -exec mv {}.new {} \;`

осталась только одно маленькое черное пятнышко, которое сильно расстраивала мою перфекционистскую сучность - конец строки в формате dos. Этого пережить было нельзя.
На выручку пришел vim.

{% highlight PowerShell %}
:arg **/*.cpp
:argdo :set ff=unix | update
{% endhighlight %}

и все в полном ажуре! Думаю можно было обойтись без невнятных `encode` одними только средствами vim

{% highlight PowerShell %}
:arg **/*.cpp
:argdo :e ++enc=cp1251 | :set fenc=utf=8 | :set ff=unix | update
{% endhighlight %}

И не мучиться. Кстати, решение при помощи vim будет работать на всех платформах, как я понимаю.
