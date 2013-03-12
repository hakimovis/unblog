---
layout: post
title: "Интересное"
date: 2013-03-12 17:47
comments: true
categories: ruby, pattern, exceptions
---
Интересный паттерн чтобы пометить, что исключения принадлежат к определенной группе. Выглядит шаманством, но работает.

    irb(main):001:0> module SomeException; end
    => nil
    irb(main):002:0> class InterruptCaught      < SystemExit;          include SomeException end
    => InterruptCaught
    irb(main):003:0>
    irb(main):004:0* begin
    irb(main):005:1*   raise InterruptCaught
    irb(main):006:1> rescue SomeException => e
    irb(main):007:1>   puts "exception #{e.inspect}"
    irb(main):008:1> end
    exception #<InterruptCaught: InterruptCaught>
    => nil