---
title: flutter grandle error 
date: 2026-04-23
keywords: flutter, grandle error
---

從你的錯誤訊息來看，雖然你執行了 chown 指令，但問題依然存在，這主要是因為兩個原因：

1. 指令輸入錯誤 ：在你的紀錄中， sudo chown -R $(whoami) /Users/cici /flutter 中間多了一個 空格 。這導致系統試圖更改 /Users/cici （你已經擁有的目錄）和根目錄下的 /flutter （通常不存在），而沒有真正改到 /Users/cici/flutter 這個資料夾。
2. Gradle 快取鎖定 ：即便權限改了，Gradle 的 daemon 可能還在背景執行並佔用著舊的權限狀態。
請按照以下 精確的步驟 再執行一次，這次我幫你把指令合併並確保路徑正確：

### 第一步：修正 Flutter SDK 權限（請複製整行）
請在終端機執行這行（確保中間沒有多餘空格）：

```
sudo chown -R $(whoami) /Users/cici/
flutter
```
執行完後，請輸入你的 Mac 密碼。

### 第二步：清理並重設環境變數
為了確保萬無一失，我們在同一個視窗內設定好變數並清理：

```
export JAVA_HOME="/Applications/Android 
Studio 2024.app/Contents/jbr/Contents/
Home"
cd /Users/cici/flutterProj/shop
flutter clean
flutter pub get
```
### 第三步：停止所有 Gradle 背景程序（這步很重要）
有時候舊的 Gradle 程序的權限快取會卡住，請執行：

```
pkill -f gradle
```
### 第四步：重新啟動
現在再次執行：

```
flutter run
```
### 為什麼還會報錯 dev.flutter.flutter-plugin-loader？
這是因為 Flutter 在編譯 Android 時，會去讀取 SDK 內部的 Gradle 腳本（路徑在 /Users/cici/flutter/packages/flutter_tools/gradle/ ）。
Gradle 會在那個目錄下建立一個 .gradle 的隱藏資料夾來存放編譯快取。 目前的錯誤訊息： Failed to create parent directory '/Users/cici/flutter/packages/flutter_tools/gradle/.gradle' 這代表你的電腦依然不准許 Flutter 寫入資料到它自己的 SDK 目錄裡。

如果上述步驟還是不行： 這可能是因為你的 Flutter SDK 放在 /Users/cici/flutter ，但某些 macOS 的安全性設定（如系統完整保護）限制了寫入。你可以嘗試把整個 flutter SDK 資料夾移動到另一個地方（例如 /Users/cici/development/flutter ），然後更新你的 PATH 。

但首先，請務必確認 第一步 的指令路徑是完全正確的 /Users/cici/flutter 。
