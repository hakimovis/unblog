---
layout: post
title: "Странная проблема с исключением Encoding::CompatibilityError"
date: 2013-03-14 00:59
comments: true
categories: ruby ruby-on-rails exception
---
Странная штука. Вот такой код:

    xml_document = Nokogiri::XML(xml_body.strip)
    if xml_document.errors.present?
      raise ServiceError.new("Invalid XML: #{xml_document.errors.map(&:to_s)}. XML: #{xml_body}")
    end

Вызывает ошибку

    Encoding::CompatibilityError (incompatible character encodings: UTF-8 and ASCII-8BIT)

если в xml_body приходит невалидный XML с русскими буквами. Точнее, ошибку вызывает "XML: #{xml\_body}" в тексте исключения. Если ее убрать (а она там и правда не нужна), то ошибка не возникает.

