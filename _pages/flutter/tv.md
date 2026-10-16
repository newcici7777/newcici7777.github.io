---
title: Google TV設定
date: 2026-10-06
keywords: flutter, Google TV
---
// 👇 👈

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- TV 沒有觸控螢幕，不加這行的話，有些 TV 會判定不相容 -->
    <uses-feature android:name="android.hardware.touchscreen" android:required="false"/>
    <!-- 宣告支援 TV，required=false 表示手機也能裝 -->
    <uses-feature android:name="android.software.leanback" android:required="false"/>
    <!-- TV 的麥克風在遙控器上，不強制要求 -->
    <uses-feature android:name="android.hardware.microphone" android:required="false"/>

    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.RECORD_AUDIO"/>

    <application
        android:label="proj1"
        android:name="${applicationName}"
        android:icon="@mipmap/ic_launcher"
        android:banner="@drawable/banner">   <!-- TV 主畫面顯示的橫幅圖 -->
        <activity ...>
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
                <!-- 沒有這行，App 不會出現在 Google TV 的主畫面 -->
                <category android:name="android.intent.category.LEANBACK_LAUNCHER"/>
            </intent-filter>
        </activity>
        ...
```

-------------------

```
adb connect <TV的IP>:5555
adb install build/app/outputs/flutter-apk/app-release.apk
```

--------------------

```
flutter build apk --release
```

android/build/app/outputs/flutter-apk/app-release.apk

-----------------------
取得與更新專案所需的外部套件（Packages）
flutter pub get


移除套件
flutter pub remove android_tv_text_field

---------
附圖中，每一個字母都是一個按鈕，
先規劃要做到以下的第3點，需要什麼技術與套件，並且跟我討論可行方案，別先自己做。
第1.與第2.與第4.目前先暫停，只先思考第3點。

範例:c-a-p 

流程規則:
1. 一開始不能點擊p，只能點擊c或a

2. 只有中間的字母a點擊了，才能點擊p

3. 當小朋友點擊c，app語音會先念音標/k/，再點擊a，app語音念音標/æ/，然後app語音再把二個音合起來一起念/kæ/，user再點p，app語音會唸/p/，然後app語音再念cap的英文單字/kæp/

4. 當小朋友點擊a，app語音會念/æ/，再點擊p，app語音念音標/p/，然後app語音再把二個音合起來一起念/æp/，user再點c，app語音就會念/k/，然後app語音再念cap的英文單字/kæp/

----------------------
ssml:
<https://docs.cloud.google.com/text-to-speech/docs/ssml?hl=zh-tw>

把base64轉成mp3

<https://www.ipvoid.com/base64-to-mp3/>


POST https://texttospeech.googleapis.com/v1/text:synthesize?key=YOUR_API_KEY

ca
```
{
  "input": {
    "ssml": "<speak><phoneme alphabet=\"ipa\" ph=\"kæ\">ka</phoneme></speak>"
  },
  "voice": {
    "languageCode": "en-US",
    "name": "en-US-Neural2-F"
  },
  "audioConfig": {
    "audioEncoding": "MP3"
  }
}
```

```
{
  "input": {
    "ssml": "<speak><prosody rate=\"x-slow\"><phoneme alphabet=\"ipa\" ph=\"ðə\">the</phoneme></prosody></speak>"
  },
  "voice": {
    "languageCode": "en-US",
    "name": "en-US-Neural2-F"
  },
  "audioConfig": {
    "audioEncoding": "MP3"
  }
}
```

```
{
  "input": {
    "ssml": "<speak><prosody rate=\"20%\"><phoneme alphabet=\"ipa\" ph=\"ðæ\">th</phoneme></prosody></speak>"
  },
  "voice": {
    "languageCode": "en-US",
    "name": "en-US-Neural2-F"
  },
  "audioConfig": {
    "audioEncoding": "MP3"
  }
}
```

2. rate 數值的設定方式：
百分比設定（最精細）：

rate="80%"（降速 20%，比正常速度稍慢）

rate="50%"（以一半的速度慢速朗讀，非常適合用來聽清楚子音的細節）

文字關鍵字設定：

rate="slow"（慢速）

rate="x-slow"（極慢速）

<speak>Hello <phoneme alphabet="ipa" ph="wɜːld">world</phoneme>! It is indeed a beautiful <phoneme alphabet="ipa" ph="wɜːld">world</phoneme>!</speak>

IPA音表
<https://docs.cloud.google.com/text-to-speech/docs/phonemes?hl=zh-tw>

不同國家
```
{
  "input": {
    "ssml": "<speak><speak><phoneme alphabet=\"ipa\" ph=\"gɜː\">gr</phoneme></speak></speak>"
  },
  "voice": {
    "languageCode": "en-GB",
    "name": "en-GB-Neural2-A"
  },
  "audioConfig": {
    "audioEncoding": "MP3"
  }
}
```

------------------

中文
```
{
  "input": {
    "ssml": "<speak><phoneme alphabet=\"pinyin\" ph=\"le4\">樂</phoneme></speak>"
  },
  "voice": {
    "languageCode": "cmn-TW",
    "name": "cmn-TW-Wavenet-A"
  },
  "audioConfig": {
    "audioEncoding": "MP3"
  }
}
```

-------------------
加上break

```
{
  "input": {
    "ssml": "<speak><prosody rate=\"80%\"><phoneme alphabet=\"ipa\" ph=\"bə\">ka</phoneme><break time=\"50ms\"/></prosody></speak>"
  },
  "voice": {
    "languageCode": "en-GB",
    "name": "en-GB-Neural2-A"
  },
  "audioConfig": {
    "audioEncoding": "MP3"
  }
}
```
