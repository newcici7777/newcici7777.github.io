---
title: Data Binding
date: 2025-10-17
keywords: kotlin, Data Binding
---
## app的build.gradle
{% highlight groovy linenos %}
android {
    //...
    buildFeatures {
        dataBinding = true
    }
}
{% endhighlight %}

## layout
layout有特殊要求，必須是layout開頭，然後把constraintlayout包到layout裡面。
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/main"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".activity.DataBindActivity">
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
{% endhighlight %}

在`<layout></layout>`之間增加`<data></data>`，裡面有變數，型別是包裝類別java.lang.Integer，不是基本型別int。<br>
{% highlight css linenos %}
<data>
    <variable
        name="username"
        type="java.lang.String" />

    <variable
        name="age"
        type="java.lang.Integer" />
</data>
{% endhighlight %}

把TextView的`android:text`與data對映，使用`@{data變數名}`
{% highlight kotlin linenos %}
<TextView android:text="@{username}" />
{% endhighlight %}

若是Integer要轉成String
{% highlight kotlin linenos %}
<TextView android:text="@{String.valueOf(age)}"/>
{% endhighlight %}

完整xml
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
            name="username"
            type="java.lang.String" />

        <variable
            name="age"
            type="java.lang.Integer" />
        <variable
            name="user"
            type="com.example.coroutine.repository.User" />
    </data>
    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/main"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".activity.DataBindActivity">
        <TextView
            android:id="@+id/tv_username"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{username}"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            tools:text="name" />
        <TextView
            android:id="@+id/tv_age"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="120dp"
            android:text="@{String.valueOf(age)}"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/tv_username"
            tools:text="age" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
{% endhighlight %}

## binding
Prerequisites:

- [viewbinding][1]

先處理viewbinding，再把data的屬性username、age設值。<br>
{% highlight kotlin linenos %}
class DataBindActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val binding = ActivityDataBindBinding.inflate(layoutInflater)
    setContentView(binding.root)
    binding.username = "Bill"
    binding.age = 10
  }
}
{% endhighlight %}

## data binding 物件
User物件
{% highlight kotlin linenos %}
data class User (val username: String, val age:Int)
{% endhighlight %}

{% highlight css linenos %}
<data>
    <variable
        name="user"
        type="com.example.coroutine.repository.User" />
</data>
{% endhighlight %}

{% highlight css linenos %}
<TextView android:text="@{user.username}" />
<TextView android:text="@{String.valueOf(user.age)}" />
{% endhighlight %}

{% highlight kotlin linenos %}
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  val binding = ActivityDataBindBinding.inflate(layoutInflater)
  setContentView(binding.root)
  val user = User("Alice", 20)
  binding.user = user
}
{% endhighlight %}

完整xml
{% highlight kotlin linenos %}
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
            name="user"
            type="com.example.coroutine.repository.User" />
    </data>
    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/main"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".activity.DataBindActivity">
        <TextView
            android:id="@+id/tv_username"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.username}"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            tools:text="name" />
        <TextView
            android:id="@+id/tv_age"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="120dp"
            android:text="@{String.valueOf(user.age)}"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/tv_username"
            tools:text="age" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
{% endhighlight %}


[1]: {% link _pages/kotlin/viewbinding.md %}
