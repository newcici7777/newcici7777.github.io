---
title: ViewBinding
date: 2025-10-15
keywords: kotlin, ViewBinding
---
## build.gradle設置
app下的build.gradle設置以下內容，設置完後一定要<span class="markline">同步Sync</span>。<br>
{% highlight groovy linenos %}
android {
    ...

    buildFeatures {
        viewBinding = true
    }
}
{% endhighlight %}

import
{% highlight kotlin linenos %}
import com.example.coroutine.databinding.ActivityMainBinding
{% endhighlight %}

## binding檔案
Binding 類名直接對應 Layout 檔案名：<br>

|Binding 類  | 對應的 Layout 檔案|
|ActivityMainBinding| activity_main.xml|
|FragmentMyBinding|   fragment_my.xml|
|ItemListBinding| item_list.xml|
|DialogCustomBinding| dialog_custom.xml|

命名規則：
移除底線 _

駝峰命名法（每個單字首字母大寫）

結尾加上 Binding

### binding放置位置
要先Run > Run app 才會在app目錄下產生 build 目錄。<br>

![img]({{site.imgurl}}/kotlin/binding.png)<br>

## Activity viewbinding步驟
### binding變數
lateinit是延後初始化，就不用給binding先設值。
{% highlight kotlin linenos %}
private lateinit var binding : ActivityMainBinding
{% endhighlight %}

### inflate 取得binding
{% highlight kotlin linenos %}
binding = ActivityMainBinding.inflate(layoutInflater)
{% endhighlight %}

點擊layoutInflater就會導到Activity中getLayoutInflater()方法。
{% highlight kotlin linenos %}
public class Activity {
...
    public final LayoutInflater getLayoutInflater() {
        if (mLayoutInflater == null) {
            return performGetLayoutInflater(null);
        }
        return mLayoutInflater;
    }
...
}
{% endhighlight %}

### 設定layout
root就是ActivityMainBinding的getRoot()的方法，找到xml第一個元素，取得view。<br>
{% highlight kotlin linenos %}
setContentView(binding.root)
{% endhighlight %}

androidx.constraintlayout.widget.ConstraintLayout就是root。<br>
{% highlight css linenos %}
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.MainActivity07">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Add"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

### 使用元件
{% highlight kotlin linenos %}
binding.textView.text = "Test"
{% endhighlight %}

### 完整程式碼
{% highlight kotlin linenos %}
class MainActivity13 : AppCompatActivity() {
  // 延後初始化
  private lateinit var binding : ActivityMainBinding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
    binding.textView.text = "Test, Test"
  }
}
{% endhighlight %}

## Fragment viewbinding步驟
### 建立xml
先在 res/layout/ 目錄下，建立一個fragment_my.xml<br>
{% highlight html linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/name_tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

### 自動產生
app下的build.gradle設置以下內容
{% highlight groovy linenos %}
android {
    ...

    buildFeatures {
        viewBinding = true
    }
}
{% endhighlight %}

Build > CleanProject

接著會自動產生 FragmentMyBinding

這個類是根據你的 XML 佈局檔案名稱自動生成的：

|你的 XML 檔案|   自動生成的 Binding 類|
|:-----|:--------|
|fragment_my.xml | FragmentMyBinding|
|fragment_home.xml | FragmentHomeBinding|
|fragment_profile.xml |FragmentProfileBinding|

命名規則：

移除底線 _

每個單字首字母大寫

結尾加上 Binding

### Fragment View Binding
會自動產生 FragmentMyBinding
```
import com.example.coroutine.databinding.FragmentMyBinding
```
使用延遲初始化
{% highlight kotlin linenos %}
private lateinit var binding: FragmentMyBinding
{% endhighlight %}

初始化binding。<br>
傳回binding的xml第一個元素root。<br>
{% highlight kotlin linenos %}
    binding = FragmentMyBinding.inflate(layoutInflater)
    return binding.root
{% endhighlight %}

Binding程式碼
{% highlight kotlin linenos %}
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
}
{% endhighlight %}

onViewCreated()使用Binding。<br>
{% highlight kotlin linenos %}
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
    binding.nameTv.text = "Hello world"
  }
}
{% endhighlight %}

### Activity使用Fragment
使用FragmentContainerView<br>
![img]({{site.imgurl}}/kotlin/fragment.png)<br>

activity_main2.xml
{% highlight html linenos %}
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="4dp"
        android:text="TextView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.52"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/fragmentContainerView3"
        android:name="com.example.coroutine.fragment.FragA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView2" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

#### binding 檔案
activity_main2.xml會產生 ActivityMain2Binding 類別

{% highlight kotlin linenos %}
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.example.coroutine.databinding.ActivityMain2Binding

class MainActivity14 : AppCompatActivity() {
  private lateinit var binding : ActivityMain2Binding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMain2Binding.inflate(layoutInflater)
    setContentView(binding.root)
    val textv = binding.textView2

  }
}
{% endhighlight %}

