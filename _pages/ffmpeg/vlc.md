---
title: vlc
date: 2025-04-02
keywords: vlc
---

```
killall VLC

# 删除 VLC 配置和缓存
rm -rf ~/Library/Preferences/org.videolan.vlc
rm -rf ~/Library/Application\ Support/org.videolan.vlc
```

啟動vlc
```
/Applications/VLC.app/Contents/MacOS/VLC -vvv "/Users/cici/Downloads/test_video.mp4" \
--sout '#rtp{sdp=rtsp://:8555/test.sdp,mux=ts}' \
--no-sout-audio
```




```
--input-repeat=999999 \  
```
試圖增加這串指令，但會讓ffplay無法連上

在vlc的playlist中要播放，以下才有效
測試vlc
```
ffplay rtsp://localhost:8555/test.sdp
```
192.168.8.103
port8554跟android虛擬機有關，因為會發現vlc日誌一直讀取android studio的內容，建議換其它的port
```
日誌一直讀取'/Users/cici/./AndroidStudioProjects/MyApplication2/app/build/intermediates/compile_and_runtime_not_namespaced_r_class_jar/debug/R.jar'
[00007fc01200bf80] main stream debug: using stream_filter module "record"
[00007fc0117265d0] main input source debug: creating demux: access='file' demux='any' location='/Users/cici/./AndroidStudioProjects/MyApplication2/app/build/intermediates/compile_and_runtime_not_namespaced_r_class_jar/debug/R.jar' file='/Users/cici/./AndroidStudioProjects/MyApplication2/app/build/intermediates/compile_and_runtime_not_namespaced_r_class_jar/debug/R.jar'
[00007fc01200c140] main demux debug: looking for demux module matching "any": 55 candidates
[00007fc01210c540] main xml reader debug: looking for xml reader module matching "any": 1 candidates
[00007fc01210c540] main xml reader debug: using xml reader module "xml"
```

192.168.8.114:5004/test

