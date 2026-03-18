---
title: pip pycharm 第三方套件
date: 2026-03-18
keywords: Python, pip3, pycharm , third party package
---
## 終端機安裝
以下適用於Mac。

打開終端機，輸入pip3，主要使用的指令有install,uninstall,list。
```
% pip3

Usage:   
  pip3 <command> [options]

Commands:
  install                     Install packages.
  lock                        Generate a lock file.
  download                    Download packages.
  uninstall                   Uninstall packages.
  freeze                      Output installed packages in requirements format.
  inspect                     Inspect the python environment.
  list                        List installed packages.
```

安裝第三方套件
```
pip3 install requests
```

列出安裝清單
```
% pip3 list
Package            Version
------------------ ---------
certifi            2026.2.25
charset-normalizer 3.4.6
idna               3.11
pip                25.3
requests           2.32.5
urllib3            2.6.3
```

移除第三方套件
```
% pip3 uninstall requests
Found existing installation: requests 2.32.5
Uninstalling requests-2.32.5:
  Would remove:
    /Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/requests-2.32.5.dist-info/*
    /Library/Frameworks/Python.framework/Versions/3.14/lib/python3.14/site-packages/requests/*
Proceed (Y/n)? y
  Successfully uninstalled requests-2.32.5
```

## Pycharm

![img]({{site.imgurl}}/python/pycharm_package1.png)<br>

選擇 `+` ，安裝時視窗底部會有一個安裝進度條。<br> 
![img]({{site.imgurl}}/python/pycharm_package2.png)<br>

輸入要安裝的第三方套件。<br>
![img]({{site.imgurl}}/python/pycharm_package3.png)<br>

測試一下是否能import 第三方套件。<br>
![img]({{site.imgurl}}/python/pycharm_package4.png)<br>

刪除第三方套件，移除時視窗底部會有一個移除進度條。<br>
![img]({{site.imgurl}}/python/pycharm_package5.png)<br>


