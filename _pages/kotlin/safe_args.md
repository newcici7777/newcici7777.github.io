---
title: NavController
date: 2025-11-14
keywords: kotlin, NavController
---
## Project 的 build.gradle
![img]({{site.imgurl}}/kotlin/safe_args.png)

專案的build.gradle，在`plugins{}`上方，加上`buildscript {}`的內容。<br>
{% highlight groovy linenos %}
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
    dependencies {
        def nav_version = "2.9.5"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }
}
plugins {
    .....
}
{% endhighlight %}

## app 的 build.gradle
在app中的build.gradle，`plugins {}`加上`id 'androidx.navigation.safeargs.kotlin'`
{% highlight groovy linenos %}
plugins {
    id 'androidx.navigation.safeargs.kotlin'
}
{% endhighlight %}

在app中的build.gradle，`dependencies {}`中加上implementation
{% highlight groovy linenos %}
dependencies {
    def nav_version = "2.9.5"

    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version"
}
{% endhighlight %}

注意 以下這行不能加在app中的build.gradle
```
implementation "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
```

## settings.gradle
settings.gradle，要確保是google()，中間沒加奇奇怪怪的東西。<br>
{% highlight groovy linenos %}
pluginManagement {
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.name = "coroutine"
include ':app'

{% endhighlight %}

注意！！！！接下來要Sync<br>

## res目錄下的navigation
注意！homeFrag是沒有參數，只有action。<br>
fragA才有argument。<br>

navigation1.xml
{% highlight kotlin linenos %}
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/navigation1"
    app:startDestination="@id/homeFrag">
    <fragment
        android:id="@+id/homeFrag"
        android:name="com.example.coroutine.fragment.HomeFrag"
        android:label="HomeFrag">
        <action
            android:id="@+id/action_homeFrag_to_fragA"
            app:destination="@id/fragA" />
    </fragment>
    <fragment
        android:id="@+id/fragA"
        android:name="com.example.coroutine.fragment.FragA"
        android:label="FragA">
        <argument
            android:name="username"
            android:defaultValue="unknow"
            app:argType="string" />
        <argument
            android:name="address"
            android:defaultValue="unknow"
            app:argType="string" />
    </fragment>
</navigation>
{% endhighlight %}

## Fragment
HomeFrag 傳送參數

{% highlight kotlin linenos %}
val action = HomeFragDirections.actionHomeFragToFragA(username = "abcd", address = "dddd")
findNavController().navigate(action)
highlight %}

完整程式碼
{% highlight kotlin linenos %}
package com.example.coroutine.fragment

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.coroutine.databinding.FragmentHomeBinding

class HomeFrag : Fragment()  {
  private lateinit var binding: FragmentHomeBinding
  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    binding = FragmentHomeBinding.inflate(layoutInflater)
    return binding.root
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    binding.navBtn1.setOnClickListener {
      val action = HomeFragDirections.actionHomeFragToFragA(username = "abcd", address = "dddd")
      findNavController().navigate(action)
    }
  }
}
{% endhighlight %}

FragA 接收參數
{% highlight kotlin linenos %}
val args = FragAArgs.fromBundle(requireArguments())
val username = args.username
val address = args.address
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
package com.example.coroutine.fragment

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.example.coroutine.databinding.FragmentMyBinding

class FragA: Fragment() {
  private lateinit var binding: FragmentMyBinding
  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    binding = FragmentMyBinding.inflate(layoutInflater)
    return binding.root
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    val args = FragAArgs.fromBundle(requireArguments())
    val username = args.username
    val address = args.address
    binding.nameTv.text = "username = $username address = $address"
  }
}
{% endhighlight %}