---
layout: post
title:  "GitHub blog with Jekyll"
date:   2021-07-10
tags: [Jekyll,GitHub]
categories: Jekyll
---

為了寫一篇新的 blog 開始研究支援 Markdown 格式的方案，最後就選了 GitHub + Jekyll 的方式，順便把過程記錄下來。

## 名詞
- [Ruby](https://www.ruby-lang.org/)  
Jekyll 是基於 Ruby 語言所撰寫的
- [Gem](https://rubygems.org/)  
Ruby 的套件管理服務
- [bundle](https://bundler.io/)  
Gem 的套件版本管理
- [Liquid](https://shopify.github.io/liquid/)  
Jekyll 所使用的模板語言(Template Language)

## Step

1. New Repository  
Repository name: [knight720].github.io  
> [GitHub Pages](https://pages.github.com/)

2. Install Jekyll  
> [Jekyll on Windows](https://jekyllrb.com/docs/installation/windows/)  

- 問題  
```
ridk install
~~~~
    CategoryInfo          : SecurityError: (:) [], PSSecurityException
    FullyQualifiedErrorId : UnauthorizedAccess
```
- 解法
```
Set-ExecutionPolicy RemoteSigned
```
> [PowerShell 執行原則](https://docs.microsoft.com/zh-tw/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.1#powershell-execution-policies)

3. Create a new Jekyll site  
> [Quickstart](https://jekyllrb.com/docs/)  

4. Preview  
```
bundle exec jekyll serve
```

- 問題
```
jekyll serve
C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/bundler-2.2.27/lib/bundler/definition.rb:496:in `materialize': Could not find minima-2.5.1, jekyll-feed-0.15.1, tzinfo-1.2.9, tzinfo-data-1.2021.1, wdm-0.1.1, webrick-1.7.0, jekyll-seo-tag-2.7.1, thread_safe-0.3.6, listen-3.5.1, ffi-1.15.3-x64-mingw32 in any of the sources (Bundler::GemNotFound)
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/bundler-2.2.27/lib/bundler/definition.rb:234:in `specs_for'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/bundler-2.2.27/lib/bundler/runtime.rb:18:in `setup'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/bundler-2.2.27/lib/bundler.rb:149:in `setup'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/plugin_manager.rb:52:in `require_from_bundler'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/exe/jekyll:11:in `<top (required)>'
        from C:/Ruby30-x64/bin/jekyll:23:in `load'
        from C:/Ruby30-x64/bin/jekyll:23:in `<main>'
```

- 解法
```
bundle install
```

## Problem
```
gem install eventmachine --platform ruby
```

```
bundle add webrick
```