---
title: Github 版本下載
date: 2026-06-17
keywords: github , version download
---
一、 在 GitHub 網頁上要怎麼選擇下載版本？
當你進入任何一個開源專案的 GitHub 首頁時，請把目光移到網頁的右側邊欄（Right Sidebar），你會看到一個區塊叫做 Releases（發佈版本）。

步驟 1：進入 Releases 頁面
點擊 Releases 區塊後，你會看到由新到舊排列的所有歷史版本。每個版本都會有版號（例如 v0.30.9、v0.1.48），並附帶詳細的更新日誌（Release Notes）。

![img]({{site.imgurl}}/git/git_release1.png)<br>

步驟 2：展開 Assets（資產/安裝包）
在每一個版本的底端，都隱藏著一個叫做 Assets 
的下拉選單（通常預設是展開的）。這裡才是真正藏著各個作業系統安裝檔的地方。

![img]({{site.imgurl}}/git/git_release2.png)<br>

你會看到一堆檔案，這時候要根據你的作業系統來選：

Mac 專用：檔名通常帶有 -darwin、mac、macOS 或 .dmg。

Windows 專用：檔名通常帶有 windows、.exe 或 .zip。

Linux 專用：檔名通常帶有 linux、-amd64 或 .tar.gz。

💡 秘密就在這裡：你看到的那個網址 .../v0.1.48/Ollama-darwin.zip，就是我在 v0.1.48 版本的 Assets 列表裡，對著 Ollama-darwin.zip 按滑鼠右鍵選擇「複製連結網址」得來的！

二、 我是怎麼知道要下載「v0.1.48」這個特定版本的？
我並不是亂猜的，這背後有一套標準的技術排查邏輯：

1. 關鍵線索：2024 年 7 月的架構大改版
我知道 Ollama 官方在 2024 年 7 月左右（大約是 v0.1.48 之後，進入 v0.2.x、v0.3.x 時代），為了支援最新、最快的 AI 模型，大幅重構了底層的運算核心（llama-server）。

官方為了追求極致的效能，在新版的編譯中，預設強制啟用了現代 CPU 的高級加速指令集（如 AVX2 的特殊優化擴充）。

2. 交叉比對你的 Mac 規格
剛才你的終端機噴出了這個致命的錯誤：

error: done_getting_tensors: wrong number of tensors; expected 147, got 146 以及 abort trap

這兩行字在舊款 Intel Mac 上是一個非常經典的「代溝特徵」：

新版軟體：認識新模型（Llama 3.2 預期有 147 個 tensor）。

新版核心（隱藏在後台的 runners）：因為你的 CPU 太舊、不支援新指令集，導致它在計算時直接被作業系統「中斷（Abort）」，算到一半少了一個 tensor（變成 146 個）。

3. 找出「最後的相容黃金期」
既然新版的核心強制要求高規格的 CPU 指令集，那解決方法就是找架構大改版之前的最後一個穩定版本。

在 Ollama 的歷史紀錄中，v0.1.48 就是那個分水嶺：

它對舊款 Intel CPU 有極佳的相容性，不強求高級指令集（所以你的 CPU 跑起來不會 Abort）。

它同時又是 0.1 世代的最成熟版本。

4. 網址的邏輯拼接
知道版號是 v0.1.48，且知道 Mac 舊版的包裝檔名叫做 Ollama-darwin.zip 後，GitHub 的下載網址公式就是固定的：
https://github.com/【作者】/【專案】/releases/download/【版號】/【檔案名稱】

拼接起來，就成了這行神奇的下載指令了！

所以，工程師在選擇版本時，通常不是選「最新」，而是選「在你的硬體限制下，能跑的最穩定版本」。這也是為什麼 GitHub 要把所有歷史版本都保留下來的原因喔！