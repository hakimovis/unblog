---
layout: post
title: "URL quote под рукой"
date: 2013-08-08 13:25
comments: true
categories: sublime-text plugin url quote
---

Плагин для Sublime Text 3 для быстрого URL quote (превращение "строка" в "%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B0").
Здесь используется urllib.parse.quote из python3. Для python2 надо использовать что-то другое.

Tools -> New Plugin ...

``` python
import sublime, sublime_plugin
from urllib.parse import quote

class UrlQuoteString(sublime_plugin.TextCommand):
    def run(self, edit):
        selection = self.view.sel()[0]
        string_to_quote = self.view.substr(selection)
        result = quote(string_to_quote)
        self.view.replace(edit, selection, result)
```

Сохраняем как url_quote_string.py и назначаем кнопки ctrl+shift+q: Preferences -> Key Bindings - User
``` python
[
    { "keys": ["ctrl+shift+q"], "command": "url_quote_string" }
]

```

Должно работать.