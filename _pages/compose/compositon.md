---
title: Compositon Local
date: 2025-12-11
keywords: Android, Jetpack compose, Compositon Local
---
## 自己寫provider
1. 宣告全域變數，有預設值。
2. 提供provider函式，參數value為要修改全域變數color與要執行的Lambda
3. 在provider函式的範圍內，全域color變成參數value，並執行參數Lambda。
4. 離開provider()函式，全域變數要改回預設值。

![img]({{site.imgurl}}/compose/provider1.png)<br>
{% highlight kotlin linenos %}
// 1.宣告全域變數，有預設值。
var color = Color.Blue
@Composable
fun test() {
  Column {
    // 字為藍色
    MyText()
    provider(Color.Red) {
      // 字為紅色
      MyText()
    }
    // 字為藍色
    MyText()
  }
}

@Composable
fun MyText() {
  Text("Provider Test", color = color)
}

// 2. 提供provider函式，參數為要修改全域變數的value與要執行的Lambda
@Composable
fun provider(value: Color, content: @Composable () -> Unit) {
  // 先暫存全域變數的值
  val temp = color
  // 全域變數color的值變成參數value
  color = value
  // 呼叫Lambda
  content()
  // 全域變數color的值恢復原況
  color = temp
}
{% endhighlight %}

## 使用Compositon Local
1. 宣告data class，成員變數color預設為Blue
2. 使用compositionLocalOf儲存data class
3. 靜態類別ColorItems，成員變數color分別為Red、Yellow
4. MyText()函式中，Text的color是目前(current)的color
5. testComposition()，第一個MyText使用預設color = Blue，只有在CompositionLocalProvider()函式的範圍內，MyText 使用參數傳來的yellow
6. 離開CompositionLocalProvider()函式，color又變成預設Blue。

![img]({{site.imgurl}}/compose/provider2.png)<br>

### provides為infix函式

- [infix][1]

{% highlight kotlin linenos %}
public infix fun provides(value: T): ProvidedValue<T> = defaultProvidedValue(value)
{% endhighlight %}

可使用空白代替.
```
原本 = LocalColor.provides(ColorItems.yellow)
空白 = LocalColor provides ColorItems.yellow
```

### 完整程式碼
{% highlight kotlin linenos %}
// 1. 宣告data class，成員變數color預設為Blue
data class MyColor(val color: Color = Color.Blue)
// 2. 使用compositionLocalOf儲存data class
val LocalColor = compositionLocalOf { MyColor() }
// 靜態類別ColorItems，成員變數color分別為Red、Yellow
object ColorItems {
  val red: MyColor = MyColor(Color.Red)
  val yellow: MyColor = MyColor(Color.Yellow)
}

@Composable
fun MyText(str: String = "", mycolor: Color = LocalColor.current.color) {
  // 4. MyText()函式中，Text的color是目前(current)的color
  Text(str, color = mycolor)
}

@Composable
fun testComposition() {
  Column {
    // 第一個MyText使用預設color = Blue
    MyText("abcd")
    // 在CompositionLocalProvider()函式的範圍內，MyText 使用參數傳來的yellow
    CompositionLocalProvider(LocalColor provides ColorItems.yellow) {
      MyText("abcd")
    }
    // 離開CompositionLocalProvider()函式
    // 使用預設color = Blue
    MyText("abcd")
    CompositionLocalProvider(LocalColor provides ColorItems.red) {
      MyText("abcd")
    }
  }
}
{% endhighlight %}



[1]: {% link _pages/kotlin/exten_func.md %}#to-infix
