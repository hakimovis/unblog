---
layout: post
title: "Stash: pull request из консоли"
date: 2013-03-13 01:23
comments: true
categories: ruby git atlassian stash pull-request
---
Вот такой способ делать пулл-реквесты с консоли для Atlassian Stash: [http://blogs.atlassian.com/2012/11/stash-pull-requests-from-the-command-line/](http://blogs.atlassian.com/2012/11/stash-pull-requests-from-the-command-line/)

И если кратко:

    $ gem install atlassian-stash
    $ stash pull-request myBranch master @michael

создает pull request из ветки myBranch в master с ревьюером "michael". Или

    $ stash pull-request master

создает pull request из текущей ветки в master. И после небольших шаманств можно будет писать:

    $ git create-pull-request master



