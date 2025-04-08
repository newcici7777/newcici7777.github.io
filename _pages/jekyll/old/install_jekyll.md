---
title: 安裝Gem jekyll
date: 2025-03-24
keywords: jekyll
---
gem install jekyll  
不能出現  
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions

千萬不能輸入sudo  
sudo gem install jekyll  
錯誤畫面  
![img]({{site.imgurl}}/jekyll/old/jekyll1.png)  
若出現以上畫面，代表ruby是舊的  
可以輸入which ruby跟ruby -v來檢查  
舊的ruby如下  
![img]({{site.imgurl}}/jekyll/old/jekyll2.png)  
請參考如何更改舊ruby路徑的文章  

確認ruby的版本都正確  
確認版本的指令如下(參考下圖)  
ruby -v  
which ruby  
然後執行gem install jekyll  
![img]({{site.imgurl}}/jekyll/old/jekyll3.png)  
回到user目錄  
輸入  
jekyll new myblog  
cd myblog  
![img]({{site.imgurl}}/jekyll/old/jekyll4.png)  
若按照官網的作法輸入  
jekyll serve  
會產生以下的錯誤  
```
/Users/cici/.rbenv/versions/3.2.2/lib/ruby/3.2.0/bundler/resolver.rb:290:in `raise_not_found!': Could not find gem 'minima (~> 2.5)' in locally installed gems. (Bundler::GemNotFound)
```
![img]({{site.imgurl}}/jekyll/old/jekyll5.png)  
參考此篇
<https://stackoverflow.com/questions/39384024/jekyll-cannot-find-gem-after-bundle-install>
進入myblog  
輸入bundle install  
![img]({{site.imgurl}}/jekyll/old/jekyll6.png)  
再輸入bundle exec jekyll serve
![img]({{site.imgurl}}/jekyll/old/jekyll7.png)  
再瀏覽器輸入  
http://localhost:4000/  
![img]({{site.imgurl}}/jekyll/old/jekyll8.png)  