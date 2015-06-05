---
published: true
title: VimTouch и встраивание plugins
excerpt: VimTouch замечательная реализация vim для AndroidOS. Один из неприятных минусов - встраиваемые плагины при помощи специального приложения. Как установить собственные plugins?
icon: fa fa-plug
autor: O Alex
date: 2015-06-02
modified: 2015-06-02
layout: post
categories: [articles]
tags: [vim, VimTouch, plugin, android, CCTools]
comments: true
image:
    feature:
share: true
---

## Суть проблемы.

Как я уже [писал](http://oalex.appplantation.com/articles/%D0%A0%D0%B5%D0%B4%D0%B0%D0%BA%D1%82%D0%BE%D1%80%D1%8B%20%D0%B8%20ide/%D0%A0%D0%B5%D1%86%D0%B5%D0%BF%D1%82%D1%8B%20%D0%BF%D1%80%D0%B8%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F%20vim/2015/05/05/Android-programming.html){:target=\"_blank\"} для программирования на платформе android я выбрал следующую конфигурацию: VimTouch+SGit+Hacker's Keyboard (все ссылки на указанные программы есть ыв статье).
Конечно есть еще не менее замечательный комплекс программ [CCTools](https://play.google.com/store/apps/details?id=com.pdaxrom.cctools&hl=ru), но идея была все-таки использовать кастомизированный под android редактор, который позволит производительно кодить. В любом случае, CCTools - это крутая подборка программ для разработки. Никто другой не предоставляет столько компиляторов и библиотек для работы как CCTools. Поговорим об этом как-нибудь потом. Сейчас речь идет о VimTouch.

<!-- more -->

## Кучка нытья

Все было бы замечательно с этой реализацией vim, если б не столкнулся с проблемой установки нужных мне плагинов. Вернее захотелось установить [snippMate](https://github.com/dspe/My-vim-config/blob/master/after/plugin/snipMate.vim). Идея использовать snippets на мобильном устройстве лежит таки на поверхности. Однако, попытки установить плагин на мобилу или планшет закончились полным фиаско.

Дело усугубляет то, что Android - сильно извращенный linux. У каждой проги на девайсе свой "HOME", причем, без root'а не доступный. В VimTouch, конечно же сделана возможность редактирования .vimrc, но этого оказалось недостаточно. Впрочем, и с root все равно неприятности остаются. Отчаянная попытка дописать runtimepath к успеху не привела. В инете в блогах жалобные вопросы "Как установить свой плагин для VimTouch" остаются без ответа. Уже начали приходить суидсидоидальные мысли пересобрать VimTouch или собрать свой AndroidVim (скажем, AVim ;)). 

## Идея решения

На помощь пришло решение от [Vundle](https://github.com/gmarik/Vundle.vim){:target=\"_blank\"} - плагина, который менеджерит другие плагины. Там как раз реализована идея расширения runtimepath для возможности расположения каждого плагина в изолированном каталоге. Может быть удасться расположить эти каталоги где-нибудь в доступном месте, например, на sdcard? И о да! Это оказалось возможным! 
   
На самом деле, обладая правами root, каталоги с плагинами можно расположить в пространстве VimTouch, но греет идея, что все можно сделать без рутирования, да и остается перспектива доработки функций обновления и установки новых плагинов. Да, пока эти проблемы не решены...

## Имплиментация

Будем считать, что вы уже умеете устанавливать [Vundle](https://github.com/gmarik/Vundle.vim){:target=\"_blank\"}. Ничего сложного нет, но требуется git. Таким образом, основная проблема оказалась скрытой в отсутствии git на устройстве android. Git из поставки CCTools не виден и скрыт в пространстве CCTools. То же самое и с SGit. Думаю можно как-то решить эту проблему, но видимо это произойдет позже, если вообще надо.

Итак, для установки потребовался комп с ubuntu 14.04. Думаю, подайдет вообще любой, на котором установлен vim и набор плагинов из под [Vundle](https://github.com/gmarik/Vundle.vim){:target=\"_blank\"}. Берем соответствующий каталог с плагинами и копируем его на карточку мобильного устройства. У меня это каталог: `.vim/bundle`. На Android получилось `/storage/sdcard0/.vim/bundle`. Думаю, что можно предварительно удалить все ненужные плагины ручками. Мне не понадобилось, у меня пока только snippMate.

Далее выбираем в меню VimTouch пункт `Edit vimrc` и добавляем в него необходимые строки:

{% highlight HTML lineos %}
" The commands in this are executed when the GUI is started.
"
" Maintainer:    Alex O <o.alex.dev@gmail.com>
" Last change:    2015-06-02
"
" Special for VimTouch (Android)
"

"------------------ Vundle ----------------
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=/storage/sdcard0/.vim/bundle/Vundle.vim
call vundle#begin('/storage/sdcard0/.vim/bundle')
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.vim'

"---# SnipMate #---
Plugin 'MarcWeber/vim-addon-mw-utils'
Plugin 'tomtom/tlib_vim'
Plugin 'garbas/vim-snipmate'

" Optional:
Plugin 'honza/vim-snippets'

" assuming you want to use snipmate snippet engine
"ActivateAddons vim-snippets snipmatets'
:imap <C-J> <Plug>snipMateNextOrTrigger
:smap <C-J> <Plug>snipMateNextOrTrigger
"--- end SnipMate ---

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
"Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
"Plugin 'L9'
" Git plugin not hosted on GitHub
"Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
"Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
"Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Avoid a name conflict with L9
"Plugin 'user/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
"------------------ end Vundle ------------

set number
set statusline=%F%m%r%h%w\ %y\ enc:%{&enc}\ [ff=%{&ff}]\ fenc:%{&fenc}\ %=(ch:%3b\ hex:%2B)\ [col:%2c\ line:%2l/%L]\ [%2p%%]

set laststatus=2
set encoding=utf8
set encoding=utf-8     " set charset translation encoding
set termencoding=utf-8 " set terminal encoding
set fileencoding=utf-8 " set save encoding
set fileencodings=utf-8,cp1251,koi8r,cp866,ucs-2le

set linebreak
set dy=lastline
set ch=1        " Make command line two lines high
set hlsearch
set incsearch
" Размер отступов
set shiftwidth=4
" Размеры табуляций
set tabstop=4
set softtabstop=4
" Более "умные" отступы при вставке их с помощью tab.
" На самом деле заметить влияние этой опции тяжело, но хуже из-за нее не будет :)
set smarttab
set expandtab

imap [ []<LEFT>
imap ( ()<LEFT>
imap { {}<LEFT>

" по звездочке не прыгать на следующее найденное, а просто подсветить
nnoremap * *N
" выключить подсветку: повесить на горячую клавишу Ctrl-F8
nnoremap <C-F8> :nohlsearch<CR>
map <C-8> :nohlsearch<CR>
" в визуальном режиме по команде * подсвечивать выделение
vnoremap * y :execute ":let @/=@\""<CR> :execute "set hlsearch"<CR>

set backupdir=/storage/sdcard0/tmp

autocmd BufRead,BufNewFile *.md set filetype=md
{% endhighlight %}

Я привожу полный vimrc которым пользуюсь. Там есть мапирование функциональных кнопок но как их использовать в VimTouch додумывайте сами. Добавлять нужно строчки отмеченные пометкой "Vundle" (строки с по)

Обратить внимание следует на следующие строки:

Строка №

`set rtp+=/storage/sdcard0/.vim/bundle/Vundle.vim` - тут следует прописать соответствующий каталог на вашей карточке.

Строка №

`call vundle#begin('/storage/sdcard0/.vim/bundle')` - и тут надо прописать нужный каталог. Без указания альтернативного каталога ничего работать не захотело.

Строка №

`autocmd BufRead,BufNewFile *.md set filetype=md` - понадобилась, чтобы объяснить vim'у, что файл в котором я сейчас пишу эту заметку с расширением `*.md` имеет тип `md`. Это необходимо для того чтобы заработал набор sniplets из файла md.sniplets . Часть кода сниплетов из него показано ниже

{% highlight VIM lineos %}
snippet img
	![${1:altText}](${2:SRC})

snippet link
	[${1:desc}](${2:HREF})

snippet link
	[${1:desc}](${2:HREF}){:target=\"_blank\"}

snippet fig
	<figure>
		<a href="${1:IMG}"><img alt="${2:Text}" title="$2" src="$1"></a>
		<figcaption>$2</figcaption>
	</figure>


snippet figh
	<figure class="half">
		<a href="${1:IMG}"><img alt="${2:Text}" title="$2" src="$1"></a>
		<figcaption>$2</figcaption>
	</figure>

snippet title
	---
	published: true
	title: ${1:TITLE}
	excerpt: ${2:DESC}
	icon: fa fa-plug
	date: `strftime("%Y-%m-%d")`
	modified: `strftime("%Y-%m-%d")`
	layout: post
	categories: [articles]
	tags: []
	comments: true
	image:
		feature:
	share: true
	---

{% endhighlight %}

Вот, собственно, и всё. Заложена база для повышения эффективности разработки на мобильных устройствах. Как минимум, посты стало писать проще.

Осталась проблема удобного обновления и инсталляции новых плагинов, но, надеюсь она разрешится в ближайшем будущем. 

## При решении задачи были использованы следующие материалы:

[Vundle. Менеджер плагинов для Vim](http://habrahabr.ru/post/148549/){:target=\"_blank\"}
[gmarik/Vundle.vim](https://github.com/gmarik/Vundle.vim){:target=\"_blank\"}



