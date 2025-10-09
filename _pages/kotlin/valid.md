---
title: 驗證函式
date: 2025-10-08
keywords: kotlin, check(), assert(), require()
---
## check
check 會丟出IllegalStateException
```
check(condition) { "錯誤訊息" }
```
condition: 你要檢查的條件（必須是 Boolean）

大括號 {} 中是當條件不成立時要顯示的錯誤訊息（可省略）

{% highlight kotlin linenos %}
val list = emptyList<Int>()
check(list.isNotEmpty()) { "List 不可以是空的!" }
{% endhighlight %}
```
IllegalStateException: List 不可以是空的!
```

{% highlight kotlin linenos %}
val list = listOf(1, 2, 3)
check(list.isNotEmpty()) { "List 不可以是空的!" }
println("List OK!")
{% endhighlight %}
```
List OK!
```

## assert()
在測試環境中才會檢查，正式環境中，就會略過assert()。<br>
assert會丟出AssertionError。<br>
assert不符合condition的條件，則會拋出AssertionError。大括號內容{}可以省略。<br>
```
assert(condition) { "錯誤訊息" }
```
{% highlight kotlin linenos %}
  fun checkAge(age: Int) {
    assert(age > 0) { "年齡要大於0" }
    println("age = $age")
  }

  @Test
  fun assertTest() {
    checkAge(0)
  }
{% endhighlight %}
```
年齡要大於0
java.lang.AssertionError: 年齡要大於0
```

## require()
require會丟出IllegalArgumentException。<br>
require不符合condition的條件，則會拋出IllegalArgumentException。大括號內容{}可以省略。<br>
```
require(condition) { "錯誤訊息" }
```
{% highlight kotlin linenos %}
  fun checkAge2(age: Int) {
    require(age > 0) { "年齡要大於0" }
    println("age = $age")
  }

  @Test
  fun requireTest() {
    checkAge2(-41)
  }
{% endhighlight %}
```
年齡要大於0
java.lang.IllegalArgumentException: 年齡要大於0
```

## 三者比較

|函式|	用途|	例外類型|	何時啟用|	用於|
|require()|	檢查「函式輸入」是否合法	IllegalArgumentException|	永遠啟用|	外部輸入參數|
|check()|	檢查「內部狀態」是否正確	IllegalStateException|	永遠啟用|	程式邏輯狀態|
|assert()|	偵錯用的邏輯檢查|	AssertionError|	只在 -ea 啟用時生效|	開發、測試階段|