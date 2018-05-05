---
layout: post
title: "解决运行jekyll serve时报错"
date: 2018-05-04
description: "解决运行jekyll serve时报错"
tag: 系统
---

﻿错误描述：

> /Library/Ruby/Gems/2.3.0/gems/bundler-1.16.1/lib/bundler/runtime.rb:313:in check_for_activated_spec!': You have already activated public_suffix 3.0.2, but your Gemfile requires public_suffix 3.0.0. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)

解决办法：

```
$ bundle exec jekyll serve
```

> $ bundle exec jekyll serve
/cygdrive/c/Users/mochapenguin/.rvm/gems/ruby-2.4.0/gems/bundler-1.14.6/lib/bundler/runtime.rb:40:in `block in setup': You have already activated json 2.0.2, but your Gemfile requires json 1.8.6.  Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
