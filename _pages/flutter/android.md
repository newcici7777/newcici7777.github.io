---
title: android
date: 2026-10-05
keywords: flutter, android
---

android/settings.gradle<br>
修改plugins中
```
plugins {
  id "com.android.application" version "8.7.3" apply false
  id "org.jetbrains.kotlin.android" version "2.1.0" apply false
}
```

android/settings.gradle 修改後的結果:
```
pluginManagement {
    def flutterSdkPath = {
        def properties = new Properties()
        file("local.properties").withInputStream { properties.load(it) }
        def flutterSdkPath = properties.getProperty("flutter.sdk")
        assert flutterSdkPath != null, "flutter.sdk not set in local.properties"
        return flutterSdkPath
    }()

    includeBuild("$flutterSdkPath/packages/flutter_tools/gradle")

    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}

plugins {
    id "dev.flutter.flutter-plugin-loader" version "1.0.0"
    id "com.android.application" version "8.7.0" apply false
    id "org.jetbrains.kotlin.android" version "2.1.0" apply false
}

include ":app"
```

------------------------------

android/build.gradle 檔案:

```
allprojects {
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.buildDir = "../build"
subprojects {
    project.buildDir = "${rootProject.buildDir}/${project.name}"
}
subprojects {
    project.evaluationDependsOn(":app")
}

tasks.register("clean", Delete) {
    delete rootProject.buildDir
}
```

---------------------------------

android/gradle/wrapper/gradle-wrapper.properties：
```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-8.10.2-all.zip
```

---------------------

android/app/build.gradle 檔案中，修改`android{ }` 裡面的內容
```
android {
	.
	.
	.
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    .
    .
    .
}
```


android/app/build.gradle 完整內容
```
plugins {
    id "com.android.application"
    id "kotlin-android"
    // The Flutter Gradle Plugin must be applied after the Android and Kotlin Gradle plugins.
    id "dev.flutter.flutter-gradle-plugin"
}

def localProperties = new Properties()
def localPropertiesFile = rootProject.file("local.properties")
if (localPropertiesFile.exists()) {
    localPropertiesFile.withReader("UTF-8") { reader ->
        localProperties.load(reader)
    }
}

def flutterVersionCode = localProperties.getProperty("flutter.versionCode")
if (flutterVersionCode == null) {
    flutterVersionCode = "1"
}

def flutterVersionName = localProperties.getProperty("flutter.versionName")
if (flutterVersionName == null) {
    flutterVersionName = "1.0"
}

android {
    namespace = "com.example.proj1"
    compileSdk = flutter.compileSdkVersion
    ndkVersion = flutter.ndkVersion

    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    defaultConfig {
        // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
        applicationId = "com.example.proj1"
        // You can update the following values to match your application needs.
        // For more information, see: https://docs.flutter.dev/deployment/android#reviewing-the-gradle-build-configuration.
        minSdk = flutter.minSdkVersion
        targetSdk = flutter.targetSdkVersion
        versionCode = flutterVersionCode.toInteger()
        versionName = flutterVersionName
    }

    buildTypes {
        release {
            // TODO: Add your own signing config for the release build.
            // Signing with the debug keys for now, so `flutter run --release` works.
            signingConfig = signingConfigs.debug
        }
    }
}

flutter {
    source = "../.."
}
```

-------------------------

android/local.properties 中flutter.sdk 路徑要注意

```
sdk.dir=/Users/cici/Library/Android/sdk
flutter.sdk=/Users/cici/flutter-3.27.4
flutter.buildMode=debug
flutter.versionName=1.0.0
flutter.versionCode=1
```

-------------------------


在專案根目錄proj1/下建一個 `.vscode` 資料夾，前面有一個點。目前這個資料夾還不存在。
在 .vscode 裡建一個 `settings.json`，內容如下：

```
{
  "dart.flutterSdkPath": "/Users/cici/flutter-3.27.4"
}
```

------------------

查看flutter版本

```
flutter --version
```


---------

vscode重新打開

```
flutter clean
flutter pub get
flutter run
```