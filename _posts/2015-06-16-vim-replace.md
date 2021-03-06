---
published: true
title: Поиск и замена в vim
excerpt: Полезные команды для поиска и замены в замечательном редакторе vim
icon: fa fa-thumbs-up
date: 2015-06-16
modified: 2015-06-16
layout: post
categories: [articles]
tags: [vim, search, replace, поиск, замена]
comments: true
image:
    feature:
share: true
---

Общий вид команды поиска и замены в текущем буфере:

`%s/pattern/replace/g`

При работе с выделенным словом (или текстом) можно выделить визуальный блок и произвести замену только в нем:

`:'<,'>s//replace/g`

Правила работы с регулярными выражениями можно найти в статье [Особенности при написании регулярных выражений в vim](/articles/Рецепты%20приготовления%20vim/2015/05/21/vim-regexp-replace.html)

<!-- more -->

В случае, когда поиск и замену необходимо выполнить сразу в нескольких файлах, следует использовать команды:

`:argdo` - обработать все файлы в списке аргументов

`:bufdo` - все буферы

`:tabdo` - все табы

`:windo` - все окна в текущем табе

Команда:

`:arddo %s/pattern/replace/ge | update`

произведет замену во всех файлах из списка аргументов и сохранит их. В случае, если ничего не найдено, то сообщение об этом будет подавлено суфиксом `ge`

`:arg` - посмотреть список аргументов

`:arg **/*.cpp` - сформировать список аргументов (показан рекурсивный обход текущего каталога)

`:argadd **/.*h` - добавить в список аргументов
