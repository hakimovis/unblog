---
layout: post
title: "Плагин для Sublime Text 2 для автозамены старого стиля хэшей на новый"
date: 2013-06-07 14:06
comments: true
categories: sublime-text plugin ruby hash
---

Вот такой просненький плагин для Sublime Text 2 помогает заменять старый стиль хэшей Ruby {:foo => 'bar'} на новый {foo: 'bar'}

``` python
import sublime, sublime_plugin
import re

class RubyNewHashStyle(sublime_plugin.TextCommand):
    def run(self, edit):
        selection = self.view.sel()[0]
        code_block = self.view.substr(selection)
        result = re.sub(r':([a-z\d_]+) =>', r'\1:', code_block)
        self.view.replace(edit, selection, result)
```

Чтобы повесить его на хоткей, в Preferences, Key Bindings - User вставляем:
``` python
[
 { "keys": ["ctrl+shift+h"], "command": "ruby_new_hash_style" }
]
```

Теперь можно просто выделить фрагмент кода с описанием хэша, нажать ctrl+shift+h и он заменится на новый:

``` ruby
  item = {:title => 'Зонтик', :status => 'Новый', :price => '400', :order => 'В наличии'}
  # Заменится на:
  item = {title: 'Зонтик', status: 'Новый', price: '400', order: 'В наличии'}
```