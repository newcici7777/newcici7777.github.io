---
title: 如何更改舊ruby路徑
date: 2025-03-24
keywords: jekyll, ruby
---
參考此篇
<https://stackoverflow.com/questions/51126403/you-dont-have-write-permissions-for-the-library-ruby-gems-2-3-0-directory-ma>
```
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc    這一步最重要官網沒提到
```
![img]({{site.imgurl}}/jekyll/old/ruby_path1.png) 
檢查一下ruby在那
![img]({{site.imgurl}}/jekyll/old/ruby_path2.png) 
以下是錯誤的位置
![img]({{site.imgurl}}/jekyll/old/ruby_path3.png) 
以下是正確的位置
![img]({{site.imgurl}}/jekyll/old/ruby_path4.png) 

