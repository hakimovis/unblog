---
layout: post
title: "Простой grep для SublimeText"
date: 2014-06-11 16:47
comments: true
categories: python sublimetext plugin grep
---

Чтобы легко выделять куски текста в логах с одним и тем же тегом (например, request guid) можно использовать простой плагин:

``` python
import sublime, sublime_plugin

class GrepCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        selected_text = self.view.substr(self.view.selection[0])
        regexp = r"^(.*){0}(.*)$".format(selected_text)
        lines = self.view.find_all(regexp)
        self.view.sel().clear()
        for line in lines: self.view.sel().add(line)
```

Сохраняем его как grep.py, назначаем сочетание клавиш ctrl+shift+g, выделяем тег, жмем это сочетание и получаем выделенными все строки, содержащие этот тег.