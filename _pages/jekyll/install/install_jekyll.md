---
title: 安裝 Jekyll
date: 2025-04-10
layout: post
keywords: jekyll
---
## 參考的教學
<https://www.youtube.com/watch?v=D9CLhQdLp8w>

## 建立Repository
![img]({{site.imgurl}}/jekyll/jekyll_install1.png)

1. 如果想在自己的根目錄可以看到網站，以下的方框輸入`你的帳號.github.io`  
2. 如果只想要在原本的github網址後面多個目錄，例:`https://你的帳號.github.io/你建立的目錄名/`，以下的方框輸入目錄名 
3. 記得要public  

![img]({{site.imgurl}}/jekyll/jekyll_install2.png)

## 使用git desktop clone下來
![img]({{site.imgurl}}/jekyll/jekyll_install3.png)

![img]({{site.imgurl}}/jekyll/jekyll_install4.png)

## 建立Gemfile
clone下來後，在clone的目錄下建立`Gemfile`

Gemfile
```
source 'https://rubygems.org'
gem 'webrick'
gem 'nokogiri'
gem 'rack', '~> 2.2.4'
gem 'rspec'
gem "jekyll", "~> 3.9.5"
gem "github-pages", group: :jekyll_plugins
gem "jekyll-include-cache", group: :jekyll_plugins
```

## 建立_config.yml
在clone的目錄下建立`_config.yml`
```
title: Jekyll Gitbook
remote_theme: sighingnow/jekyll-gitbook
plugins:
    - jekyll-paginate
    - jekyll-sitemap
    - jekyll-gist
    - jekyll-feed
    - jekyll-include-cache
```

## 建立README.md
README.md
```
內容隨便寫
```

## 指令
打開終端機，輸入以下指令
```
% bundle update 
% gem install
% bundle install
% git add Gemfile.lock
% bundle
% bundle exec jekyll serve
```

將產生以下畫面
```
% % bundle exec jekyll serve
/usr/local/lib/ruby/gems/3.3.0/gems/jekyll-3.9.5/lib/jekyll.rb:28: warning: csv was loaded from the standard library, but will no longer be part of the default gems since Ruby 3.4.0. Add csv to your Gemfile or gemspec. Also contact author of jekyll-3.9.5 to add csv into its gemspec.
Configuration file: /Users/cici/GitHub/old/_config.yml
To use retry middleware with Faraday v2.0+, install `faraday-retry` gem
            Source: /Users/cici/GitHub/old
       Destination: /Users/cici/GitHub/old/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
      Remote Theme: Using theme sighingnow/jekyll-gitbook
       Jekyll Feed: Generating feed for posts
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 2.77 seconds.
 Auto-regeneration: enabled for '/Users/cici/GitHub/old'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
      Regenerating: 1 file(s) changed at 2025-04-10 12:41:32
                    README.md
      Remote Theme: Using theme sighingnow/jekyll-gitbook
       Jekyll Feed: Generating feed for posts
                    ...done in 0.096139 seconds.
```

打開瀏覽器輸入網址
```
http://127.0.0.1:4000
```

## 上傳到你的github
commit > push

## Github pages
進入Github pages  
![img]({{site.imgurl}}/jekyll/jekyll_install5.png)

設置branch，並save  
![img]({{site.imgurl}}/jekyll/jekyll_install6.png)

到Action查看佈屬狀況，黃色代表正在佈屬  
![img]({{site.imgurl}}/jekyll/jekyll_install7.png)

綠色代表佈屬成功
![img]({{site.imgurl}}/jekyll/jekyll_install8.png)

## 查看網站
二種方式:  
- https://你的帳號.github.io/  
- https://你的帳號.github.io/你建立的目錄名/  