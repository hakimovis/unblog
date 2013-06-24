---
layout: post
title: "Тестирование Rake-тасков"
date: 2013-06-24 12:23
comments: true
categories: rake-task rails rspec test
---

Вот тут [http://robots.thoughtbot.com](http://robots.thoughtbot.com/post/11957424161/test-rake-tasks-like-a-boss) подсмотрел более-менее красивый способ тестирования rake-тасков с помощью rspec. Сначала создадим вспомогательный файл:
``` ruby
# spec/support/shared_contexts/rake.rb
require "rake"

shared_context "rake" do
  let(:rake)      { Rake::Application.new }
  let(:task_name) { self.class.top_level_description }
  let(:task_path) { "lib/tasks/#{task_name.split(":").first}" }
  subject         { rake[task_name] }

  def loaded_files_excluding_current_rake_file
    $".reject {|file| file == Rails.root.join("#{task_path}.rake").to_s }
  end

  before do
    Rake.application = rake
    Rake.application.rake_require(task_path, [Rails.root.to_s], loaded_files_excluding_current_rake_file)
    Rake::Task.define_task(:environment)
  end

  def exec_rake_task
    subject.invoke
  end
end
```

Теперь можно писать сами спеки, назвая блок describe именем rake-таска:
``` ruby
require 'spec_helper'

describe 'world:destroy' do
  include_context 'rake'

  it 'creates instance of WorldDestroyer and calls #destroy! method' do
    WorldDestroyer.any_instance.should_receive(:destroy!)
    exec_rake_task
  end

end
```
