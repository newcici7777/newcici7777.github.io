---
title: Glide
date: 2025-12-01
keywords: kotlin, Glide
---
## builde.gradle(app)
{% highlight groovy linenos %}
dependencies {
    implementation "com.github.bumptech.glide:glide:4.16.0"
}
{% endhighlight %}

## 使用Glide
準備ImageView
{% highlight css linenos %}
    <ImageView
        android:id="@+id/image1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:srcCompat="@tools:sample/avatars" />
{% endhighlight %}

Activity載入ImageView
{% highlight kotlin linenos %}
    val image1 = findViewById<ImageView>(R.id.image1)
{% endhighlight %}

載入圖片
{% highlight kotlin linenos %}
    Glide.with(this)
      .load("圖片網址")
      .into(image1)
{% endhighlight %}

## 自訂options
自訂大小與loading圖片，網路連線錯誤的圖片。
{% highlight kotlin linenos %}
    val options = RequestOptions()
      .placeholder(R.drawable.load)
      .error(R.drawable.error)
      .override(100,100)

    Glide.with(this)
      .load("圖片網址")
      .apply(option)
      .into(image1)      
{% endhighlight %}

把options移到擴展函式，寫一個GlideExten.kt，內容如下:
{% highlight kotlin linenos %}
import com.bumptech.glide.RequestBuilder
import com.example.coroutine.R

fun <T> RequestBuilder<T>.loadAvatar(): RequestBuilder<T> {
  return this
    .circleCrop()
    .placeholder(R.drawable.load)
    .error(R.drawable.error)
    .override(100, 100)
}
{% endhighlight %}

把.apply()，替換成自己寫的擴展函式.loadAvatar()
{% highlight kotlin linenos %}
    val options = RequestOptions()
      .placeholder(R.drawable.load)
      .error(R.drawable.error)
      .override(100,100)

    Glide.with(this)
      .load("圖片網址")
      .loadAvatar()
      .into(image1)      
{% endhighlight %}

## 圖片圓角
```
transform(CircleCrop())  // 圖片變成圓圈
transform(RoundedCorners(80))  // 圖片四個角度為80
transform(GranularRoundedCorners(30f,80f,80f,30f))  // 設定四個角度，數字後面f，浮點數
```

{% highlight kotlin linenos %}
    Glide.with(this)
      .load("圖片網址")
      .apply(options)
      //.transform(CircleCrop())
      //.transform(RoundedCorners(80))
      .transform(GranularRoundedCorners(30f,80f,80f,30f))
      .into(image1)
{% endhighlight %}

## 交叉淡入
建立一個DrawableCrossFadeFactory，使用.transition()，設定淡入。
{% highlight kotlin linenos %}
    val factory = DrawableCrossFadeFactory
      .Builder(300)
      .setCrossFadeEnabled(true)
      .build()

    Glide.with(this)
      .load("圖片網址")
      .apply(options)
      .transition(DrawableTransitionOptions.withCrossFade(factory))
      .into(image1)      
{% endhighlight %}

現代的Kotlin不建議使用GlideApp，一律使用擴展函式。
