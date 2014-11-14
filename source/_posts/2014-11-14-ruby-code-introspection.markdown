---
layout: post
title: "Ruby code introspection"
date: 2014-11-14 12:00
comments: true
categories: ruby code-introspection method
---

В языке Ruby есть возможность получить метод объекта как объект:

``` ruby
> Math.method(:sin)
=> #<Method: Math.sin>
```

Этот объект можно передавать в другие методы как блок:

``` ruby
> sin_method = Math.method(:sin)
=> #<Method: Math.sin>
> (1..10).map(&sin_method)
=> [0.8414709848078965, 0.9092974268256817, 0.1411200080598672, -0.7568024953079282, -0.9589242746631385, -0.27941549819892586, 0.6569865987187891, 0.9893582466233818, 0.4121184852417566, -0.5440211108893699]
```

Также, у метода можно получить название класса или модуля, где он был определен и путь к нему:

``` ruby
> Task.new.method(:valid?).owner
=> ActiveRecord::Validations
> Task.new.method(:valid?).source_location
=> ["/usr/local/lib/ruby/gems/2.1.0/gems/activerecord-4.2.0.beta2/lib/active_record/validations.rb", 55]
```

С помощью гема [method_source](https://github.com/banister/method_source) можно даже получить исходный код метода:

``` ruby
> puts Task.new.method(:valid?).source
    def valid?(context = nil)
      context ||= (new_record? ? :create : :update)
      output = super(context)
      errors.empty? && output
    end
=> nil
```

С помощью метода ```#comments``` можно получить комментарии к методу.

[Источник](http://www.justinweiss.com/blog/2014/11/10/fun-with-the-method-method/?utm_source=rubyweekly&utm_medium=email)

