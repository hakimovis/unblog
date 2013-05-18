---
layout: post
title: "Добавляем генератор миграции и моделей к своему гему"
date: 2013-05-18 12:57
comments: true
categories: ruby gem rails generator
---

В продолжение предыдущей темы расскажу как научить свой гем использовать генератор rails для добавления своей модели в проект:

``` bash
$ rails generate install_my_gem

      create  db/migrate/20120724092434_create_my_models.rb
      create  app/models/my_model.rb
```

Во первых, создадим сами файлы миграции и модели. Для удобства это лучше делать в традиционных для рельсового прокта местах. Все-таки их придется тестировать. Во вторых, создадим файл install_my_gem.rb ("install_my_gem" можно заменить на название вашего генератора) в папке lig/generators/install_my_gem:

``` ruby
class InstallMyGemGenerator < Rails::Generators::Base
  source_root MyGem::GEM_PATH # В этой константе содержится путь к установленному гему
  # Все методы, объявленные ниже будут выполняться по порядку по команде rails generate install_my_gem

  def copy_migration
    # Копируем файл миграции из своего db/migrate в тот же db/migrate проекта
    migration_file = '20120724092434_create_my_models.rb'
    copy_file "db/migrate/#{migration_file}", "db/migrate/#{migration_file}"
  end

  def copy_my_model
    # Аналогично копируем файл проекта
    copy_file 'app/models/my_model.rb', 'app/models/my_model.rb'
  end
end
```

MyGem::GEM_PATH нужно будет объявить где-нибудь в my_gem.rb:
``` ruby
module MyGem
  GEM_PATH = File.expand_path('..', File.dirname(__FILE__))
  ...
```


