---
layout: post
title: "Использование и тестирование моделей ActiveRecord в своем геме"
date: 2013-05-18 12:38
comments: true
categories: gem ruby ruby-on-rails active-record rspec
---

Если ruby gem должен приносить с собой модель, возникнет вопрос - как же его тестировать. Разумеется, подымать для прогона тестов вашего гема базу данных, навешивать миграции и конфигурить конфиги никто не будет, поэтому есть замечательная штука - sqlite3-адаптер для ActiveRecord, работающий с базой данных прямо в памяти. Чтобы воспользоваться этим не нужно ничего особо хитрого.

Сначала добавим зависимости в Gemfile:
``` ruby
group :test do
  # Т.к. в реальном проекте гем activerecord уже будет подключен и база данных будет своя,
  # а не sqlite, подключаем их только для группы :test
  gem 'activerecord'
  gem 'sqlite3'
end
```

и подгружаем их же в spec_helper.rb:

``` ruby
require 'active_record'
require 'sqlite3'
```

Создаем в spec/support файл active_record.rb с примерно таким содержанием:

``` ruby
require 'rspec/rails/extensions/active_record/base' # Всяческие хелперы для тестирования.

# Тут указываем, что будем использовать адаптер sqlite3 и базу данных размещаем в памяти.
ActiveRecord::Base.establish_connection adapter: "sqlite3", database: ":memory:"

# Запускаем миграции из папки db/migrate или любой другой, которую укажем.
ActiveRecord::Migrator.up "db/migrate"

# А это нужно, чтобы каждый тест запускался в своей транзакции, которая будет откатываться назад
RSpec.configure do |config|
  config.around do |example|
    ActiveRecord::Base.transaction do
      example.run
      raise ActiveRecord::Rollback
    end
  end
end
```

В обшем-то все. Мы можем использовать наши модели на полную катушку если, конечно, не забыли их подгрузить в spec_helper.rb:
``` ruby
Dir["#{File.dirname(__FILE__)}/../app/models/*.rb"].each { |f| require f }
```

Это очень вольный перевод вот этой статьи: [http://iain.nl/testing-activerecord-in-isolation](http://iain.nl/testing-activerecord-in-isolation)
