---
layout: post
title: "Поддержка Unicode в консоли Ruby для MacOS"
date: 2014-04-29 14:33
comments: true
categories: irb unicode ruby rails-console rbenv macos
---

Если в консоли Ruby при вводе русских букв выводится что-то вроде ``` >> \U+FFD1\U+FFD0\U+FFB5\U+FFD1\U+FFD1 ``` нужно установить readline:

```
$ brew update && brew install readline
```

И пересобрать версию Ruby с указанием того, что используем новый readline:

```
CONFIGURE_OPTS=--with-readline-dir=`brew --prefix readline` rbenv install 1.9.3-p374
```

Вместо 1.9.3-p374 нужно указать свою версию.

Взято [отсюда](https://coderwall.com/p/wdm-_q).
