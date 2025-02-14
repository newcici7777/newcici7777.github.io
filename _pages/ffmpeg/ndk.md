---
title: NDK
date: 2025-02-05
keywords: java, ndk, android
---

## 建立NDK

![img]({{site.imgurl}}/ndk/create_ndk1.png)
![img]({{site.imgurl}}/ndk/create_ndk2.png)
![img]({{site.imgurl}}/ndk/create_ndk3.png)
![img]({{site.imgurl}}/ndk/create_ndk4.png)
![img]({{site.imgurl}}/ndk/create_ndk5.png)
![img]({{site.imgurl}}/ndk/create_ndk6.png)

## 檢查SDK
NDK與CMake要有勾選
![img]({{site.imgurl}}/ndk/check_sdk.png)

## cmake
自己建立lib
![img]({{site.imgurl}}/ndk/cmake1.png)
匯入(find)其它lib,自己建立的lib與其它lib綁定，並產生鏈結檔
![img]({{site.imgurl}}/ndk/cmake2.png)



## activity

![img]({{site.imgurl}}/ndk/activity1.png)
![img]({{site.imgurl}}/ndk/activity2.png)
![img]({{site.imgurl}}/ndk/activity3.png)

## 增加jni方法
![img]({{site.imgurl}}/ndk/create_jni1.png)
![img]({{site.imgurl}}/ndk/create_jni2.png)
![img]({{site.imgurl}}/ndk/create_jni3.png)


##下載NDK r28
歷經千辛萬苦，終於ndk build ffmpeg成功， 以下為編譯後的重要記事作為記錄。

MAC下載ndk後，請選xxxxxxxx.app，然後滑鼠"右鍵" > "顯示套件內容" > "Contents" > "NDK"

把NDK目錄複製到家目錄下(或任何你能記住的地方)


### LLVM
出現一些編譯問題，首先cross-prefix要使用llvm，因為NDK r28已經移除aarch64-linux-android-strip
統一使用 LLVM 提供的 llvm-strip，所以cross-prefix一律由llvm來處理bin相關執行檔。
{% highlight c++ linenos %}
--cross-prefix=$TOOLCHAIN/bin/llvm- \
{% endhighlight %}

### SYSROOT
extra-cflags一律簡化成只有sysroot參數
{% highlight c++ linenos %}
  --extra-cflags="--sysroot=$SYSROOT" \
{% endhighlight %}

### cpu
{% highlight c++ linenos %}
--cpu=armv8-a
{% endhighlight %}

### nm
nm由LLVM處理，因為NDK r28已經移除aarch64-linux-android-nm
{% highlight c++ linenos %}
--nm=$TOOLCHAIN/bin/llvm-nm \
{% endhighlight %}

### 腳本權限

```
chmod 777 腳本檔名.副檔名
```
或
```
chmod +x 腳本檔名.副檔名
```

### config.log

檢查 ffbuild/config.log 文件，使用error或failed等關鍵字搜尋錯誤訊息
```
grep "error" config.log
```

### make V=1
V=1查看詳細的編譯過程
```
make -j4 V=1
```

### sysroot
```
  --sysroot=$SYSROOT \
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