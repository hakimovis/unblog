---
layout: post
title: "Удаление дубликатов по двум полям"
date: 2013-03-25 16:16
comments: true
categories: postgresql unique index
---

Если забыть вовремя добавить составной уникальный индекс в таблицу, там обязательно образуются дубликаты, после которых добавить такой индекс уже будет нельзя. Например, для таблицы favorites добавляем уникальный индекс на поля user_id и name. Получим подобное:

    PG::Error: ERROR:  could not create unique index "index_favorites_on_user_id_and_name"
    DETAIL:  Key (user_id, name)=(1, foo) is duplicated.
    : CREATE UNIQUE INDEX "index_favorites_on_user_id_and_name" ON "favorites" ("user_id", "name")

Чтобы все-таки добавить этот индекс, придется руками удалить дубликаты. Скорее всего, удалять их все не стоит, оставим только по одному. Это делается примерно таким запросом:

    DELETE FROM favorites WHERE id NOT IN (
      SELECT MIN(id) FROM favorites GROUP BY (name, user_id)
    );

Делается выборка каждого первого id из групп name + user_id. Их мы как раз оставляем, а удаляем все, которые не попали в выборку.