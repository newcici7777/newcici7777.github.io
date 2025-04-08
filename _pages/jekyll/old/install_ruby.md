---
title: 安裝Ruby
date: 2025-03-24
keywords: ruby
---
使用rbenv
<https://github.com/rbenv/rbenv>

建議安裝rbenv  
因為參考Jekyll官網的安裝方法會出現許多問題  
ERROR:  Error installing jekyll:  
ERROR: Failed to build gem native extension.  
![img]({{site.imgurl}}/jekyll/old/ruby1.png) 

或
ERROR:  While executing gem ... (Gem::CommandLineError)  
    Unknown command jekyll  
或  
ERROR:  While executing gem ... (Gem::FilePermissionError)  
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.  
參考以下文章  
<https://www.rubyonmac.dev/error-installing-jekyll-failed-to-build-gem-native-extension>


安裝方法，參考  
<https://github.com/rbenv/rbenv>

輸入
```
brew install rbenv ruby-build
```
![img]({{site.imgurl}}/jekyll/old/ruby2.png) 

```
sudo apt install rbenv
```

```
rbenv init
```

選擇要安裝的版本
```
rbenv install -l
```
![img]({{site.imgurl}}/jekyll/old/ruby3.png) 
安裝3.2.2
```
rbenv install 3.2.2
```

```
rbenv global 3.2.2  
rbenv local 3.2.2   
```

```
gem install bundler
```

```
gem env home
```

echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc    這一步最重要官網沒提到

參考如何更改舊ruby路徑