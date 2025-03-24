---
title: FFmpeg on Android
date: 2025-03-23
keywords: android, ffmpeg
---
Prerequisites:
- [查詢模擬器CPU架構][1]

## 編譯ffmpeg
### 下載ndk
歷經千辛萬苦，終於ndk build ffmpeg成功， 以下為編譯後的重要記事作為記錄。   
MAC下載ndk後，請選xxxxxxxx.app，然後滑鼠"右鍵" > "顯示套件內容" > "Contents" > "NDK"  
把NDK目錄複製到家目錄下(或任何你能記住的地方)

### 建立build_android.sh檔案並修改權限
建立build_android.sh在ffmpeg目錄下  
修改權限，增加執行功能
```
chmod 777 build_android.sh
```
或
```
chmod +x build_android.sh
```
### 交叉編譯過程
在ffmpeg目錄下執行以下指令
1. 終端機執行`./build_android.sh`
2. 會自動去執行./configure，後面有設置許多編譯時需要用到的參數。
3. 會自動產生makefile檔案
4. build_android.sh自動執行`make install`指令
5. 至OUTPUT參數的路徑查看是否有產生動態庫.so或靜態庫.a

### make V=1
V=1查看詳細的編譯過程
```
make -j4 V=1
```
### make clean
清理

### make install
執行makefile，並把產生出來的動態庫與靜態庫放在設置OUTPUT參數的目錄下

## 編譯參數介紹
### LLVM
出現一些編譯問題，首先cross-prefix要使用llvm，因為NDK r28已經移除aarch64-linux-android-strip
統一使用 LLVM 提供的 llvm-strip，所以cross-prefix一律由llvm來處理bin相關執行檔。
{% highlight c++ linenos %}
--cross-prefix=$TOOLCHAIN/bin/llvm- \
{% endhighlight %}

### SYSROOT
表頭檔(include目錄)與lib目錄的放置路徑  
{% highlight c++ linenos %}
  --sysroot=$SYSROOT \
{% endhighlight %}

### cflags
轉給gcc編譯器的參數，參數`--sysroot=`，告訴編譯器去那個路徑尋找head file與lib
extra-cflags一律簡化成只有sysroot參數
{% highlight c++ linenos %}
  --extra-cflags="--sysroot=$SYSROOT" \
{% endhighlight %}

#### build.ninja
build.ninja是AndroidStudio交叉編譯工具  
若不知道cflag(給c++編譯器的參數)怎麼寫，可以打開Android studio，參考下圖的路徑，找到你要編譯出來的andorid作業系統，找到build.ninja，參考flags參數
![img]({{site.imgurl}}/ndk/build_ninja.png)
-D 意思為Android Stuido交叉編譯器，所設置的前置指令，c++的前置指令是define  

### cpu
{% highlight c++ linenos %}
--cpu=armv8-a
{% endhighlight %}

### arch架構
要編譯那種架構的靜態庫或動態庫

### nm
nm由LLVM處理，因為NDK r28已經移除aarch64-linux-android-nm
{% highlight c++ linenos %}
--nm=$TOOLCHAIN/bin/llvm-nm \
{% endhighlight %}

### config.log
檢查ffmpeg目錄下的ffbuild/config.log文件，使用error或failed等關鍵字搜尋錯誤訊息
```
grep "error" config.log
```
### BIN_PREFIX
BIN_PREFIX最後面沒有任何`-`
```
BIN_PREFIX=$TOOLCHAIN/bin/$TARGET$API_LEVEL
```
### 增加PATH,CC,CXX
export PATH=$PATH:/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/bin
export CC=aarch64-linux-android24-clang
export CXX=aarch64-linux-android24-clang++

### ./configure
configure產生makefile的shell檔案  
什麼是makefile？指導gnu make如何編譯的shell檔案  
執行make  
會產生.so檔或.a檔  
若執行./configure沒權限，替./configure增加可執行權限  
```
chmod +x configure
```
### ./configure --help
幫助的文件

### --prefix
產生出來的.so檔或.a檔，放置的資料夾  
pwd為放置.configure檔案的所在目錄  
```
./configure --prefix=`pwd`/android/armeabi-v72 \
```
傳遞--prefix參數給configure  
`\`為換行字元，若沒這個字元，所有參數必須全在同一行，不能換行

可以一行傳多個參數，注意!最後一行沒有`\`

```
./configure --prefix=`pwd`/android/armeabi-v72 \
--xxx=abc \
--xxx=abcd -xxx=abcde \
--xxx=abcd -xxx=abcde --xx=abcdef 
```

進到已解壓的ffmpeg目錄，執行以下句子
```
./configure --help
```
會發現bindir,datadir,docdi,libdir...，目錄前面都有PREFIX，如:[PREFIX/lib]
```
Standard options:
  --logfile=FILE           log tests and output to FILE [ffbuild/config.log]
  --disable-logging        do not log configure debug information
  --fatal-warnings         fail if any configure warning is generated
  --prefix=PREFIX          install in PREFIX [/usr/local]
  --bindir=DIR             install binaries in DIR [PREFIX/bin]
  --datadir=DIR            install data files in DIR [PREFIX/share/ffmpeg]
  --docdir=DIR             install documentation in DIR [PREFIX/share/doc/ffmpeg]
  --libdir=DIR             install libs in DIR [PREFIX/lib]
  --shlibdir=DIR           install shared libs in DIR [LIBDIR]
  --incdir=DIR             install includes in DIR [PREFIX/include]
  --mandir=DIR             install man page in DIR [PREFIX/share/man]
  --pkgconfigdir=DIR       install pkg-config files in DIR [LIBDIR/pkgconfig]
  --enable-rpath           use rpath to allow installing libraries in paths
                           not part of the dynamic linker search path
                           use rpath when linking programs (USE WITH CARE)
  --install-name-dir=DIR   Darwin directory name for installed targets
```
### Configuration options
```
Configuration options:
  --disable-static         do not build static libraries [no]
  --enable-shared          build shared libraries [no]
  --enable-small           optimize for size instead of speed
  --disable-runtime-cpudetect disable detecting CPU capabilities at runtime (smaller binary)
  --enable-gray            enable full grayscale support (slower color)
  --disable-swscale-alpha  disable alpha channel support in swscale
  --disable-all            disable building components, libraries and programs
  --disable-autodetect     disable automatically detected external libraries [no]
```

--disable-static 不要產生靜態庫.a，預設是[no]，預設會產生靜態庫.a  
--enable-shared 產生動態庫.so，預設是[no]，預設不會產生動態庫.so  
--enable-small 優化lib的大小，使lib容量更小  

### Program options
```
Program options:
  --disable-programs       do not build command line programs
  --disable-ffmpeg         disable ffmpeg build
  --disable-ffplay         disable ffplay build
  --disable-ffprobe        disable ffprobe build
```
--disable-programs 不要產生ffmpeg,ffplay,ffprobe執行檔  
--disable-programs 就相當於執行以下三個  
```
  --disable-ffmpeg         disable ffmpeg build
  --disable-ffplay         disable ffplay build
  --disable-ffprobe        disable ffprobe build
```

### Component options
ffmpeg的組成
```
Component options:
  --disable-avdevice       disable libavdevice build
  --disable-avcodec        disable libavcodec build
  --disable-avformat       disable libavformat build
  --disable-swresample     disable libswresample build
  --disable-swscale        disable libswscale build
  --disable-postproc       disable libpostproc build
  --disable-avfilter       disable libavfilter build
  --disable-pthreads       disable pthreads [autodetect]
  --disable-w32threads     disable Win32 threads [autodetect]
  --disable-os2threads     disable OS/2 threads [autodetect]
  --disable-network        disable network support [no]
  --disable-dwt            disable DWT code
  --disable-error-resilience disable error resilience code
  --disable-lsp            disable LSP code
  --disable-faan           disable floating point AAN (I)DCT code
  --disable-iamf           disable support for Immersive Audio Model
  --disable-pixelutils     disable pixel utils in libavutil
```

ffmpeg主要由下面的元件所組成:  
libavformat：用於各種音視頻封裝格式的生成和「解析」，包括獲取解碼所需資訊以產生解碼上下文結構  
和讀取音視頻幀等功能；  
libavcodec ：用於各種類型「聲音/影像」編「解碼」；  
libavutil ：包含一些公共的「工具函數」；  
libswscale ：用於視訊場景比例「縮放」、色彩映射轉換；  
libpostproc ：用於後製效果處理；  
ffmpeg ：此專案提供的工具，可用於格式轉換、解碼或電視卡即時編碼等；  
ffsever ：一個HTTP 多媒體即時廣播串流伺服器；  
ffplay ：是一個簡單的播放器，使用ffmpeg 函式庫解析和解碼，透過SDL顯示；  

要disable的有下列元件 (disable就不會產生靜態庫，不會讓apk肥大) 
avdevice : 操作相機鏡頭(不支持android相機) 可以關閉  --disable-avdevice
postproc : 用於後製效果處理

不能disable的有下列元件  
- avcodec  影像聲音解碼
- avformat 影像聲音封裝格式解碼
- avfilter : 影像加上字幕加上浮水印
- swscale : 放大縮小

swresample 聲音重新採樣，音檔可能有雙聲道，若想把雙聲道變單聲道，需要用這個元件。

### Individual component options:
```
Individual component options:
  --disable-encoder=NAME   disable encoder NAME
  --enable-encoder=NAME    enable encoder NAME
  --disable-encoders       disable all encoders
  --disable-decoder=NAME   disable decoder NAME
  --enable-decoder=NAME    enable decoder NAME
  --disable-decoders       disable all decoders
```
--disable-encoder=NAME   關閉編碼，影片播放時不需要編碼  
--enable-decoder=NAME    打開解碼，影片播放時需要解碼  
--disable-muxer=NAME     關閉混合封裝  (把圖片與聲音混合在一起)，但是播放時，不需要產生影片，所以關閉。

### cross-compile
cross-compile就是交叉編譯的意思，使用各種不同作業系統的交叉編譯來編譯ffmpeg
1. --enable-cross-compile   打開交叉編譯
2. --cross-prefix=$TOOLCHAIN/bin/llvm- 設定交叉編譯的工具
所有編譯過程使用的工具gcc或link工具，前綴都是`$TOOLCHAIN/bin/llvm-`

## build_android.sh完整檔案
{% highlight c++ linenos %}
#!/bin/bash

# 設置變量
NDK=/Users/cici/NDK
API_LEVEL=24
# 可選：arm, arm64, x86, x86_64
ARCH=arm64  
OUTPUT=/Users/cici/android-build

# 根據架構設置目標三元組和目錄名
case "$ARCH" in
  arm)
    TARGET=armv7a-linux-androideabi
    ;;
  arm64)
    TARGET=aarch64-linux-android
    ;;
  x86)
    TARGET=i686-linux-android
    ;;
  x86_64)
    TARGET=x86_64-linux-android
    ;;
  *)
    echo "錯誤：不支持的架構 '$ARCH'"
    exit 1
    ;;
esac

# 工具鏈路徑
TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/darwin-x86_64
SYSROOT=$TOOLCHAIN/sysroot
BIN_PREFIX=$TOOLCHAIN/bin/$TARGET$API_LEVEL

# 檢查工具鏈是否存在
if [ ! -d "$TOOLCHAIN" ]; then
  echo "錯誤：工具鏈路徑 '$TOOLCHAIN' 不存在"
  exit 1
fi

# 檢查編譯器是否存在
if [ ! -f "$BIN_PREFIX-clang" ]; then
  echo "錯誤：編譯器 '$BIN_PREFIX-clang' 不存在"
  exit 1
fi

# 檢查輸出目錄是否存在，如果不存在則創建
if [ ! -d "$OUTPUT" ]; then
  echo "創建輸出目錄：$OUTPUT"
  mkdir -p "$OUTPUT"
fi


# 測試編譯器
echo "測試編譯器是否能生成可執行文件..."
echo 'int main() { return 0; }' > test.c
$BIN_PREFIX-clang test.c -o test

if [ $? -ne 0 ]; then
  echo "錯誤：編譯器測試失敗"
  echo "請檢查以下內容："
  echo "1. 編譯器路徑：$BIN_PREFIX-clang"
  echo "2. sysroot 路徑：$SYSROOT"
  echo "3. 環境變量：CC=$CC, CXX=$CXX, PATH=$PATH"
  exit 1
else
  echo "編譯器測試成功"
  rm test.c test
fi

echo $OUTPUT

# 配置 FFmpeg
echo "開始配置 FFmpeg..."
./configure \
  --nm=$TOOLCHAIN/bin/llvm-nm \
  --libdir=${OUTPUT}/libs/$ARCH \
  --incdir=${OUTPUT}/includes/$ARCH \
  --pkgconfigdir=${OUTPUT}/pkgconfig/$ARCH \
  --target-os=android \
  --arch=$ARCH \
  --cpu=armv8-a \
  --sysroot=$SYSROOT \
  --cross-prefix=$TOOLCHAIN/bin/llvm- \
  --cc=$BIN_PREFIX-clang \
  --cxx=$BIN_PREFIX-clang++ \
  --extra-cflags="--sysroot=$SYSROOT" \
  --extra-ldexeflags="-pie --sysroot=$SYSROOT" \
  --enable-shared \
  --enable-small \
  --disable-static \
  --disable-doc \
  --prefix=$OUTPUT

# 檢查配置是否成功
if [ $? -ne 0 ]; then
  echo "錯誤：FFmpeg 配置失敗"
  exit 1
fi

# 編譯並安裝
echo "開始編譯 FFmpeg..."
make clean
make -j4 V=1

# 檢查編譯是否成功
if [ $? -ne 0 ]; then
  echo "錯誤：FFmpeg 編譯失敗"
  exit 1
fi

echo "開始安裝 FFmpeg..."
sudo make install

# 檢查安裝是否成功
if [ $? -ne 0 ]; then
  echo "錯誤：FFmpeg 安裝失敗"
  exit 1
fi

echo "FFmpeg 編譯和安裝完成！"
{% endhighlight %}

[1]: {% link _pages/ffmpeg/cmake.md %}#查詢模擬器CPU架構