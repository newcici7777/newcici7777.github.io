---
title: gcloud CLI
date: 2026-10-14
keywords: gcloud
---

至以下網址安裝 gcloud CLI xx 版

<https://docs.cloud.google.com/sdk/docs/install-sdk?hl=zh-tw>

下載完後，把google-cloud-sdk目錄，移到家目錄，執行
```
~/google-cloud-sdk/install.sh
```

若安裝過程中，沒有把google-cloud-sdk路徑放至.zshrc，可執行以下命令
```
echo 'source ~/google-cloud-sdk/path.zsh.inc' >> ~/.zshrc
echo 'source ~/google-cloud-sdk/completion.zsh.inc' >> ~/.zshrc
```

檢查版本
```
gcloud --version
```

輸入以下指令，進行登入
```
gcloud init 
```

如果無法登入，也可以用以下的指令
```
gcloud auth application-default login
```

接下來瀏覽器打開，記得要打勾勾，然後才按「繼續」按鈕

啟用
```
gcloud services enable texttospeech.googleapis.com
```

Agent Platform API (這個要錢！)
<https://console.cloud.google.com/apis/library/aiplatform.googleapis.com?project=你的專案id>
