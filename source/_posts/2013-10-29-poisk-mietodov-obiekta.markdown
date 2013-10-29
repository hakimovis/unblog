---
layout: post
title: "Поиск методов объекта"
date: 2013-10-29 15:41
comments: true
categories: ruby code-introspection methods
---

Часто бывает нужно посмотреть, какие методы есть у объекта. Для этого существует простой метод:

```
> object = 1
> object.methods.sort
=> [:!, :!=, :!~, :%, :&, :*, :**, :+, :+@, :-, :-@, :/, :<, :<<, :<=, :<=>, :==, :===, :=~, :>, :>=, :>>, :[], :^, :__id__, :__send__, :`, :abs, :abs2, :acts_like?, :ago, :angle, :arg, :as_json, :between?, :binding_n, :blank?, :breakpoint, :byte, :bytes, :capture, :ceil, :chr, :class, :class_eval, :clone, :coerce, :conj, :conjugate, :day, :days, :debugger, :deep_dup, :define_singleton_method, :denominator, :display, :div, :divmod, :downto, :dup, :duplicable?, :enable_warnings, :encode_json, :enum_for, :eql?, :equal?, :even?, :exabyte, :exabytes, :extend, :fdiv, :find_methods, :floor, :fortnight, :fortnights, :freeze, :from_now, :frozen?, :gcd, :gcdlcm, :gem, :gigabyte, :gigabytes, :hash, :hour, :hours, :html_safe?, :i, :imag, :imaginary, :in?, :initialize_clone, :initialize_dup, :inspect, :instance_eval, :instance_exec, :instance_of?, :instance_values, :instance_variable_defined?, :instance_variable_get, :instance_variable_names, :instance_variable_set, :instance_variables, :integer?, :is_a?, :is_haml?, :kilobyte, :kilobytes, :kind_of?, :lcm, :load, :load_dependency, :magnitude, :megabyte, :megabytes, :method, :methods, :minute, :minutes, :modulo, :month, :months, :multiple_of?, :next, :nil?, :nonzero?, :numerator, :object_id, :odd?, :ord, :ordinal, :ordinalize, :petabyte, :petabytes, :phase, :polar, :pred, :presence, :present?, :pretty_inspect, :pretty_print, :pretty_print_cycle, :pretty_print_inspect, :pretty_print_instance_variables, :private_methods, :protected_methods, :psych_to_yaml, :psych_y, :public_method, :public_methods, :public_send, :quietly, :quo, :rationalize, :real, :real?, :rect, :rectangular, :remainder, :require, :require_dependency, :require_or_load, :respond_to?, :respond_to_missing?, :round, :second, :seconds, :send, :silence, :silence_stderr, :silence_stream, :silence_warnings, :since, :singleton_class, :singleton_method_added, :singleton_methods, :size, :step, :succ, :suppress, :suppress_warnings, :taint, :tainted?, :tap, :terabyte, :terabytes, :times, :to_bn, :to_c, :to_d, :to_default_s, :to_enum, :to_f, :to_formatted_s, :to_i, :to_int, :to_json, :to_param, :to_query, :to_r, :to_ruby, :to_s, :to_v8, :to_yaml, :to_yaml_properties, :truncate, :trust, :try, :try!, :uniq_methods, :unloadable, :untaint, :until, :untrust, :untrusted?, :upto, :week, :weeks, :with_options, :with_warnings, :year, :years, :zero?, :|, :~]
```

Среди такой каши из общих для всех классов методов и методов самого объекта трудно найти то, что нас заинтересует, правда? Методы вроде ```!, :!=, :!~, :<, :<=, is_a?, :is_haml?, :kind_of?, :load``` и куча других есть у каждого объекта, а нам надо именно специфичные для нашего объекта методы. Логично, что их надо отсеять. И лучше это делать для всей ruby-консоли.

Для этого открываем файл ```.irbrc```, который обычно находится в домашней папке и добавляем туда следующее:

``` ruby
# For methods introspection
class Object
  def uniq_methods
    (self.methods - Class.methods).sort
  end

  def find_methods(pattern)
    (self.methods.find_all {|m| m.to_s.include?(pattern.to_s)}).sort
  end
end
```

Теперь, для любого объекта будут доступны два метода: ```uniq_methods``` и ```find_methods```:

``` ruby
> Date.today.uniq_methods
=> [:+, :-, :<<, :>>, :acts_like_date?, :advance, :ago, :ajd, :amjd, :asctime, :at_beginning_of_day, :at_beginning_of_month, :at_beginning_of_quarter, :at_beginning_of_week, :at_beginning_of_year, :at_end_of_day, :at_end_of_month, :at_end_of_quarter, :at_end_of_week, :at_end_of_year, :at_midnight, :beginning_of_day, :beginning_of_month, :beginning_of_quarter, :beginning_of_week, :beginning_of_year, :between?, :change, :compare_with_coercion, :compare_without_coercion, :ctime, :cwday, :cweek, :cwyear, :day, :day_fraction, :days_ago, :days_since, :days_to_week_start, :default_inspect, :downto, :end_of_day, :end_of_month, :end_of_quarter, :end_of_week, :end_of_year, ...]

> Date.today.find_methods 'year'
=> [:at_beginning_of_year, :at_end_of_year, :beginning_of_year, :cwyear, :end_of_year, :last_year, :next_year, :prev_year, :year, :years_ago, :years_since]
```

Первый метод вернет методы, которые есть только у этого объекта, без общих для всех классов. Вторым методом удобно искать методы по словам. Например, выше мы нашли все методы, содержащие слово year.
