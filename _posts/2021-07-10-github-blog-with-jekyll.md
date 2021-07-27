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

3. Create a new Jekyll site  
> [Quickstart](https://jekyllrb.com/docs/)  

4. Preview  
```
bundle exec jekyll serve
```

## Problem
```
gem install eventmachine --platform ruby
```

```
bundle add webrick
```