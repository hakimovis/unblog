---
layout: post
title: "Узнайте bundler лучше"
date: 2013-03-15 22:27
comments: true
categories: ruby bundler
---
Статья про bundler, много интересного: [http://habrahabr.ru/company/engineyard/blog/172807/](http://habrahabr.ru/company/engineyard/blog/172807/)

Вкратце:  
**bundle outdated** показывает список гемов, которые можно обновить.  
**bundle show** показывает путь, куда установлен гем.  
**bundle open** открывает код гема в редакторе:  

    $ bundle open json
    To open a bundled gem, set $EDITOR or $BUNDLER_EDITOR
    $ export EDITOR=vim
    $ bundle open json
**bundle prestine** восстанавливает код указанного гема, если мы его подправляли редактором (используя bundle open)  
**bundle show --path** показывает список путей, где установлены все гемы проекта. Вроде, так можно найти файлы, где определяются интересные нам классы:

    grep -R ActionDispatch::RemoteIp `bundle show --paths`
**bundle init** создает пустой Gemfile  
Гемы можно брать из локального репозитория:

    $ echo "gem 'rack', :github => 'rack/rack', :branch => 'master'" >> Gemfile
    $ bundle config local.rack ~/sw/gems/rack
    $ bundle show rack
    /Users/andre/sw/gems/rack
В bundler 1.2 прямо в Gemfile можно объявить версию ruby. Если текущая версия не будет совпадать, получим исключение:

    ruby '1.9.3'
Можно построить визуальную схему зависимостей гемов:

    $ brew install graphviz
    $ gem install ruby-graphviz
    $ bundle viz
**bundle console** запустит ruby-консоль, но со всеми загруженными гемами, указанными в Gemfile. Удобно, если нам не нужен гем rails.

Создание заготовки для своего гема, установка его в систему и публикация на rubygems.org:

    bundle gem my\_super\_gem
    rake install
    rake release
Вот так можно запускать rake-таски без полного написания bundle exec rspec spec:

    $ bundle binstubs rspec-core
    $ bin/rspec spec
    No examples found.
