---
layout: post
title: "Ruby On Rails и загрузка изображений на Amazon S3 Storage"
date: 2013-05-12 23:23
comments: true
categories: ruby ruby-on-rails paper-clip amazon s3 upload
---

Даже не подозревал, что прикрутить аплоад картинок на амазоновый S3 окажется так просто. Добавляем гемы в Gemfile:

    gem 'paperclip'
    gem 'aws-sdk'

Делаем миграцию:

    class CreatePhotos < ActiveRecord::Migration
      def self.up
        create_table :photos do |t|
          t.boolean :enabled, null: false, default: true
          t.timestamps
        end
        add_attachment :photos, :image
      end

      def self.down
        drop_table :photos
      end
    end

Тут у модели Photo будет только два поля - image и enabled (КО). Теперь модель:

    class Photo < ActiveRecord::Base
      attr_accessible :enabled, :image
      has_attached_file(
        :image,
        styles: {medium: "500x500>", small: "50x50>"},
        storage: :s3,
        s3_credentials: "#{Rails.root}/config/s3.yml",
        bucket: 'bucket_name',
        s3_protocol: 'https',
        s3_host_name: 's3-eu-west-1.amazonaws.com'
      )
    end

Тут все в принципе ясно. Изображение будет храниться еще в двух размерах - medium (500x500) и small (50x50). Пришлось немного почесать голову над последними двумя опциями, без них Амазон будет ругаться XML-кой с просьбой указать endpoint. В s3.yml пишем свои ключи отсюда [portal.aws.amazon.com](https://portal.aws.amazon.com/gp/aws/securityCredentials#account_identifiers) (не забывая добавить этот конфиг в .gitignore):

    access_key_id: *****
    secret_access_key: ******

Вот и вся магия. Теперь в форме загрузки будем писать так:

    = form_for @photo, url: photos_path, html: {id: "photo_form", multipart: true} do |f|
      = f.file_field :image
      = f.check_box :enabled
      .actions
        = f.submit 'Сохранить'

И чтобы показать нашу картинку в среднем размере со ссылкой на оригинал в шаблонах пишем:

    %a{href: @photo.image.url}= image_tag @photo.image.url :medium

Ну не радость ли?