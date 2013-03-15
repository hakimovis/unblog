---
layout: post
title: "Про деплой с Github на сервер"
date: 2013-03-01 14:07
comments: true
categories: ['github', 'deploy', 'habrahabr']
---

На Habrahabr есть статья, где презентуется [скрипт для легкой настройки деплоя через гитхаб на сервер](http://habrahabr.ru/post/171195/). На всякий случай, запомним.

Сам я пользуюсь простым как мыло способом:

    #!/bin/sh
    echo "--- Generating static pages"
    bundle exec rake generate
    echo "--- Indexing changes"
    git add -A
    echo "--- Making commit: $1"
    git commit -m"deploy $1"
    echo "--- Pushing to master"
    git push origin master
    echo "--- Executing remote pull"
    ssh server.host.name /path/to/project/deploy_unblog.sh

И сам deploy_unblog.sh на сервере:

    #!/bin/bash
    cd /path/to/project
    git pull