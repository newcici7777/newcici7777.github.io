---
title: FFmpeg in Mac
date: 2025-02-03
keywords: c++, FFmpeg, Mac, OSX, IOS
---
## 安裝步驟
### 下載ffmpeg source code
[Download source code](https://ffmpeg.org/download.html#build-mac)

下載完後解壓，進入解壓後的目錄

### 安裝 Xcode Command Line Tools
在 macOS 下，你需要 Xcode Command Line Tools 提供的編譯工具（如 gcc 和 make）。執行以下命令來確認是否已安裝：

```
xcode-select --install
```

### 安裝gcc
```
brew install gcc
```

### 安裝yasm
```
brew install yasm
```

### 安裝ffmpeg

```
./configure --prefix=/usr/local/ffmpeg --enable-debug=3 --enable-shared --disable-static
```

安裝至`/usr/local/ffmpeg`

預設產生靜態檔，使用`--disable-static`把產生靜態檔關閉

`--enable-shared`，產生動態檔


### 編譯ffmpeg

```
make -j 4
```

### 安裝ffmpeg

```
sudo make install
```

### 目錄

切換目錄至/usr/local/ffmpeg，會有以下四個目錄

```
$ls
bin	include	lib	share
```

#### lib
進入lib目錄，檢查是否產生動態檔，動態檔副檔名為`.dylib`，副檔名`.a`為靜態檔，是我之前產生的，可以先略過。
```
% ls                                            
libavcodec.61.19.100.dylib	libavformat.dylib
libavcodec.61.dylib		libavutil.59.39.100.dylib
libavcodec.a			libavutil.59.dylib
libavcodec.dylib		libavutil.a
libavdevice.61.3.100.dylib	libavutil.dylib
libavdevice.61.dylib		libswresample.5.3.100.dylib
libavdevice.a			libswresample.5.dylib
libavdevice.dylib		libswresample.a
libavfilter.10.4.100.dylib	libswresample.dylib
libavfilter.10.dylib		libswscale.8.3.100.dylib
libavfilter.a			libswscale.8.dylib
libavfilter.dylib		libswscale.a
libavformat.61.7.100.dylib	libswscale.dylib
libavformat.61.dylib		pkgconfig
libavformat.a
```

#### include
放head file的目錄，進行ffmpeg開發時，需要include head file
```
cici@liyutingdeMacBook-Pro include % ls
libavcodec	libavfilter	libavutil	libswscale
libavdevice	libavformat	libswresample
```

#### bin
放ffmpeg執行檔
```
cici@liyutingdeMacBook-Pro bin % ls
ffmpeg	ffprobe
```

#### share
使用範例與手冊

## xcode匯入ffmpeg

### 建立Xcode Project

![img]({{site.imgurl}}/swift/create1.png)
![img]({{site.imgurl}}/swift/create2.png)
![img]({{site.imgurl}}/swift/create3.png)
![img]({{site.imgurl}}/swift/create4.png)

### 建立目錄

進入先前建立的project目錄後，再進入myapp2，指令如下
```
% cd myapp2
% ls
myapp2			myapp2.xcodeproj
% cd myapp2
% pwd
/Users/cici/projects/myapp2/myapp2
```

建立include與libs目錄
```
% mkdir include
% mkdir libs
% ls
Assets.xcassets		Preview Content		libs
ContentView.swift	include			myapp2App.swift
```

拷貝先前安裝的ffmpeg的include跟lib目錄
```
% cp -r /usr/local/ffmpeg/include/* ./include/
% cp -r /usr/local/ffmpeg/lib/* ./libs/
```

### 匯入目錄

![img]({{site.imgurl}}/swift/include1.png)
![img]({{site.imgurl}}/swift/include2.png)
![img]({{site.imgurl}}/swift/include3.png)
![img]({{site.imgurl}}/swift/include4.png)
![img]({{site.imgurl}}/swift/include5.png)

### 建立c檔案

![img]({{site.imgurl}}/swift/cfile1.png)
![img]({{site.imgurl}}/swift/cfile2.png)
![img]({{site.imgurl}}/swift/cfile3.png)
![img]({{site.imgurl}}/swift/cfile4.png)
![img]({{site.imgurl}}/swift/cfile5.png)

### 手動建立Bridging Header

xcode Version 14.2，每一個版本有不同的方式。

若先前沒有按下`Create Bridging Header`的按鈕，補救方法如下:

1. 在 Xcode，選擇 File → New → File...
2. 選擇 Header File (.h)，然後按 Next
3. 命名為 專案名稱-Bridging-Header.h（例如：MyProject-Bridging-Header.h）
4. 確保它位於你的專案內，並按 Create

![img]({{site.imgurl}}/swift/bridge1.png)

在 Build Settings 設定 Bridging Header

![img]({{site.imgurl}}/swift/bridge2.png)

### include ffmpeg header file
增加以下句子在testc.h
```
#include "libavutil/avutil.h"
```

testc.h
{% highlight c++ linenos %}
#ifndef testc_h
#define testc_h

#include <stdio.h>
#include "libavutil/avutil.h"
void printLog();
#endif /* testc_h */
{% endhighlight %}


testc.c
{% highlight c++ linenos %}
#include "testc.h"
void printLog() {
  av_log_set_level(AV_LOG_DEBUG);
  av_log(NULL, AV_LOG_DEBUG, "Hello world!");
  return;
}
{% endhighlight %}

### swift呼叫c
ViewController
{% highlight c++ linenos %}
import Cocoa

class ViewController: NSViewController {

  override func viewDidLoad() {
    super.viewDidLoad()
    self.view.setFrameSize(NSSize(width: 320, height: 240))
    let btn = NSButton.init(title: "button", target: nil, action: nil)
    btn.frame = NSRect(x: 320/2-40, y: 240/2-15, width:80, height:30)
    btn.bezelStyle = .rounded
    btn.setButtonType(.pushOnPushOff)
    btn.target = self
    btn.action = #selector(myfunc)
    self.view.addSubview(btn)
    // Do any additional setup after loading the view.
  }
  
  @objc
  func myfunc() {
    printLog();
  }
  override var representedObject: Any? {
    didSet {
    // Update the view, if already loaded.
    }
  }
}
{% endhighlight %}