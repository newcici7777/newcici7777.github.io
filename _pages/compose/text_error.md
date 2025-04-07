---
title: Compose Text Error
date: 2023-05-30
keywords: Android, Jetpack compose
---
建立Compose Text出現Error  
![img]({{site.imgurl}}/compose/text_error1.png)
出現以下錯誤  
```
2 issues were found when checking AAR metadata:

  1.  Dependency 'androidx.core:core:1.12.0-alpha03' requires libraries and applications that
      depend on it to compile against codename "UpsideDownCake" of the
      Android APIs.

      :app is currently compiled against android-33.

      Recommended action: Use a different version of dependency 'androidx.core:core:1.12.0-alpha03',
      or set compileSdkPreview to "UpsideDownCake" in your build.gradle
      file if you intend to experiment with that preview SDK.

  2.  Dependency 'androidx.core:core-ktx:1.12.0-alpha03' requires libraries and applications that
      depend on it to compile against codename "UpsideDownCake" of the
      Android APIs.

      :app is currently compiled against android-33.

      Recommended action: Use a different version of dependency 'androidx.core:core-ktx:1.12.0-alpha03',
      or set compileSdkPreview to "UpsideDownCake" in your build.gradle
      file if you intend to experiment with that preview SDK.
```
解決方法:  
把app的build.gradle中的 `androidx.core:core-ktx:+改成1.9.0`  
[https://developer.android.com/jetpack/androidx/releases/compose-kotlin?hl=zh-tw](https://developer.android.com/jetpack/androidx/releases/compose-kotlin?hl=zh-tw)  
Compose 修改成1.4.2，kotiln改成1.8.1
![img]({{site.imgurl}}/compose/text_error2.png)
![img]({{site.imgurl}}/compose/text_error3.png)
![img]({{site.imgurl}}/compose/text_error4.png)

出現Duplicate Error  
參考此篇
```
Duplicate class kotlin.collections.jdk8.CollectionsJDK8Kt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.internal.jdk7.JDK7PlatformImplementations found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk7-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.6.0)
Duplicate class kotlin.internal.jdk8.JDK8PlatformImplementations found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.io.path.ExperimentalPathApi found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk7-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.6.0)
Duplicate class kotlin.io.path.PathRelativizer found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk7-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.6.0)
Duplicate class kotlin.io.path.PathsKt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk7-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.6.0)
Duplicate class kotlin.io.path.PathsKt__PathReadWriteKt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk7-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.6.0)
Duplicate class kotlin.io.path.PathsKt__PathUtilsKt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk7-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.6.0)
Duplicate class kotlin.jdk7.AutoCloseableKt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk7-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.6.0)
Duplicate class kotlin.jvm.jdk8.JvmRepeatableKt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.random.jdk8.PlatformThreadLocalRandom found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.streams.jdk8.StreamsKt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.streams.jdk8.StreamsKt$asSequence$$inlined$Sequence$1 found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.streams.jdk8.StreamsKt$asSequence$$inlined$Sequence$2 found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.streams.jdk8.StreamsKt$asSequence$$inlined$Sequence$3 found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.streams.jdk8.StreamsKt$asSequence$$inlined$Sequence$4 found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.text.jdk8.RegexExtensionsJDK8Kt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
Duplicate class kotlin.time.jdk8.DurationConversionsJDK8Kt found in modules jetified-kotlin-stdlib-1.8.0 (org.jetbrains.kotlin:kotlin-stdlib:1.8.0) and jetified-kotlin-stdlib-jdk8-1.6.0 (org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.6.0)
```