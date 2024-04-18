---
title: 安裝 Ruby on Mac
date: 2024-04-17
layout: post
keywords: ruby,mac
---

打開終端機，輸入下面指令

```
$brew install ruby
```

檢查安裝路徑

```
$brew list ruby
```

homebrew安裝ruby的路徑列表，注意版號是否為3.0以上，目前是3.3.0。

```
/usr/local/Cellar/ruby/3.3.0/bin/bundle
/usr/local/Cellar/ruby/3.3.0/bin/bundler
/usr/local/Cellar/ruby/3.3.0/bin/erb
/usr/local/Cellar/ruby/3.3.0/bin/gem
/usr/local/Cellar/ruby/3.3.0/bin/irb
/usr/local/Cellar/ruby/3.3.0/bin/racc
/usr/local/Cellar/ruby/3.3.0/bin/rake
/usr/local/Cellar/ruby/3.3.0/bin/rbs
/usr/local/Cellar/ruby/3.3.0/bin/rdbg
/usr/local/Cellar/ruby/3.3.0/bin/rdoc
/usr/local/Cellar/ruby/3.3.0/bin/ri
/usr/local/Cellar/ruby/3.3.0/bin/ruby
/usr/local/Cellar/ruby/3.3.0/bin/syntax_suggest
/usr/local/Cellar/ruby/3.3.0/bin/typeprof
/usr/local/Cellar/ruby/3.3.0/include/ruby-3.3.0/ (191 files)
/usr/local/Cellar/ruby/3.3.0/lib/libruby.3.3.dylib
/usr/local/Cellar/ruby/3.3.0/lib/pkgconfig/ruby-3.3.pc
/usr/local/Cellar/ruby/3.3.0/lib/ruby/ (4276 files)
/usr/local/Cellar/ruby/3.3.0/lib/libruby.dylib
/usr/local/Cellar/ruby/3.3.0/libexec/gembin/ (2 files)
/usr/local/Cellar/ruby/3.3.0/share/emacs/site-lisp/ruby/ruby-style.el
/usr/local/Cellar/ruby/3.3.0/share/man/ (4 files)
/usr/local/Cellar/ruby/3.3.0/share/ri/ (15158 files)
```

檢查本機ruby的安裝的位置是否與homebrew一致。

```
$which ruby
```

我的mac顯示的位置是∶

```
/usr/bin/ruby
```

顯然是不一樣，修改環境變數。

```
$vi ~/.zshrc
```

添加homebrew 安裝ruby的路徑，注意!以下的位置是你的brew list ruby的路徑，不能直接拷貝我的。

```
export PATH="/usr/local/Cellar/ruby/3.3.0/bin:$PATH"
```

修改完.zshrc檔案，在終端機執行以下指令。

```
$source ~/.zshrc
```

檢查ruby的路徑是否為homebrew安裝的路徑。

```
$which ruby
```
