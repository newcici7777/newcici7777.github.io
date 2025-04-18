---
title: OpenSLES音訊
date: 2025-04-14
keywords: Android, openSLES
---
官網
<https://developer.android.com/ndk/guides/audio/opensl/getting-started?hl=zh-tw>

libOpenSLES放置位置
/Users/cici/NDK/toolchains/llvm/prebuilt/darwin-x86_64/sysroot/usr/lib/aarch64-linux-android/32/libOpenSLES.so

ndk的範例
<https://github.com/android/ndk-samples>

OpenSLES的ndk範例
<https://github.com/android/ndk-samples/blob/main/native-audio/app/src/main/cpp/native-audio-jni.c>

混音器
// create the engine and output mix objects
JNIEXPORT void JNICALL Java_com_example_nativeaudio_NativeAudio_createEngine