---
published: true
title: Как подключить Android mtp-устройство к Ubuntu 14.04
excerpt: Последовательность нажатия кнопок для того, чтобы на Ubuntu можно было работать с флешками андроидного устройства
icon: fa fa-android
date: 2015-07-13
modified: 2015-07-13
layout: post
categories: [articles]
tags: [android, ubuntu, linux, mtp]
comments: true
image:
    feature:
share: true
---

Написано по мотивам статьи [How-To Connect an Android device using MTP in Ubuntu 14.04 LTS](http://ubuntuforums.org/showthread.php?t=2226702){:target=\"_blank\"}

Устанавливаем необходимые для работы библиотеки:

`sudo apt-get install libmtp-common mtp-tools libmtp-dev libmtp-runtime libmtp9`

`sudo apt-get dist-upgrade`

<!-- more -->
Далее редактируем конфигурационный файл `fuse.conf` программы FUSE, которая предоставляет доступ к сервису монтирования 

`sudo gvim /etc/fuse.conf`

Нужно раскоментарить строку `user_allow_other`. Должно получиться что-то типа:

{% highlight PowerShell lineos %}

#/etc/fuse.conf - Configuration file for Filesystem in Userspace (FUSE)

#Set the maximum number of FUSE mounts allowed to non-root users.
#The default is 1000.
#mount_max = 1000

# Allow non-root users to specify the allow_other or allow_root mount options.
user_allow_other

{% endhighlight %}

Добавляем правила для подключения устройства. Для этого необходимо знать значения для idVendor и idProduct. Для их получения выполняем команду:

`lsusb`

Среди выведенных строчек необходимо найти "свою" строку. Устройство должно быть уже подключено перед этим вызовом. Если нет уверенности, можно отключить устройство и снова выполнить команду `lsusb` и далее играть в игру "найди одно отличие".

Должно получиться что-то типа:

{% highlight PowerShell lineos %}

Bus 002 Device 003: ID 0fce:01b1 Sony Ericsson Mobile Communications AB 
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 006: ID 0461:4d65 Primax Electronics, Ltd 
Bus 001 Device 005: ID 0846:9020 NetGear, Inc. WNA3100(v1) Wireless-N 300 [Broadcom BCM43231]

{% endhighlight %}

В приведенном примере искомое устройство находится в первой строчке (что совершенно случайно). Таким образом, vendor id это 0fce, а product id - 01b1

Запускаем редактор:

`sudo gvim /lib/udev/rules.d/69-mtp.rules`

Добавляем правила, скажем, в конец файла:

{% highlight PowerShell %}

# Sony Xperia Z2 Tablet
ATTR{idVendor}=="0fce", ATTR{idProduct}=="01b1", SYMLINK+="libmtp-%k", ENV{ID_MTP_DEVICE}="1", ENV{ID_MEDIA_PLAYER}="1"

{% endhighlight %}

Коментарий с сонькой конечно же лучше заменить на имя собственного устройства. Значение это не играет, но потом можно будет хотя бы понять к чему эта строчка относится через пару недель!

Значения idProduct и idVendor нужно подствить свои. Это уже принципиально.

Добавляем еще одно правило:

`sudo gvim /etc/udev/rules.d/51-android.rules`

{% highlight PowerShell %}

ATTR{idVendor}=="0fce", ATTR{idProduct}=="01b1", MODE="0666"

{% endhighlight %}

Значения idProduct и idVendor опять же подствляем свои.

После всех этих добавлений и сохранений перезапускаем сервис:

`sudo service udev restart`

В принципе, присоединенное устройство уже дложно быть видно.

После перезапуска системы

`sudo reboot`

с подключаемым устройством можно работать как с обычной флешкой.
