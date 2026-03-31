---
title: Mac OS 13 安裝dart
date: 2026-03-26
keywords: dart, install
---
我的Mac比較舊，使用舊版dart。<br>

## 安裝dart
解除舊版
```
brew unlink dart
```

安裝3.3.4
```
brew install dart-lang/dart/dart@3.3.4
```

將剛才安裝的 3.3.4 版本設定為系統預設：
```
brew link --force dart-lang/dart/dart@3.3.4
```

測試是否已更換
```
dart --version
dart test.dart
```

安裝Trae編輯器

## Trae編輯器使用方式
安裝Dart<br>
![img]({{site.imgurl}}/dart/install_dart2.png)<br>

新建檔案<br>
![img]({{site.imgurl}}/dart/install_dart1.png)<br>

執行程式碼，並輸出。<br>
![img]({{site.imgurl}}/dart/install_dart3.png)<br>

## `Run | Debug` 出不來

在dart檔案目錄下增加pubspec.yaml<br>
這個檔案會告訴 IDE：「這是一個 Dart 專案」。<br>
```
dart
  |- test.dart
  |- pubspec.yaml
```

pubspec.yaml 內容如下
```
name: my_dart_project
description: A simple Dart console application.
version: 1.0.0

environment:
  sdk: '>=3.0.0 <4.0.0'
```

重啟或重新載入 IDE．會有`Run | Debug`<br>
![img]({{site.imgurl}}/dart/install_dart4.png)<br>