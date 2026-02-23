---
title: Mac Python安裝
date: 2026-02-23
keywords: Python
---
## 環境設定
先到此網址下載並安裝python <https://www.python.org/downloads/>

打開終端機輸入

```
which python3
```

若不是顯示最新版本，設定最新版本的路徑
```
ls /Library/Frameworks/Python.framework/Versions/
```

此步驟會列出安裝的版本目錄，以我為例，安裝的版本目錄是
```
3.14
```

檢查bin目錄下是否有`python3`執行檔
```
ls /Library/Frameworks/Python.framework/Versions/3.14/bin/
```

確認有之後，設定環境變數
```
vi ~/.zshrc
```

在.zshrc最後加上以下內容，版本`3.14`是我的版本
```
export PATH="/Library/Frameworks/Python.framework/Versions/3.14/bin:$PATH"
```

再執行
```
source ~/.zshrc
```

再次檢查環境變數是否已修正
```
python3 --version
```

## Pycharm

![img]({{site.imgurl}}/python/pycharm1.png)

![img]({{site.imgurl}}/python/pycharm2.png)

![img]({{site.imgurl}}/python/pycharm3.png)

![img]({{site.imgurl}}/python/pycharm4.png)

![img]({{site.imgurl}}/python/pycharm5.png)

