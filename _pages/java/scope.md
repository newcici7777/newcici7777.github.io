---
title: 區域變數全域變數
date: 2025-09-12
keywords: java , scope
---
## 全域變數
在方法()之外宣告的變數為全域變數，生命周期跟著物件同生共死。<br>
以下為方法之外宣告的變數，也是成員屬性。<br>
{% highlight java linenos %}
public class Variable {
  int global = 100;
  public static void main(String[] args) {

  }
  
  public void func1() {

  }
}
{% endhighlight %}

## 全域變數預設值
全域變數沒有設值，也會有預設值。

|類型|預設值|
|int| 0 |
|short| 0|
|byte| 0|
|long| 0|
|float| 0.0f|
|double| 0.0|
|char| \u0000|
|boolean| false|
|String| null|

{% highlight java linenos %}
public class Variable {
  int global;
  public static void main(String[] args) {
    Variable variable = new Variable();
    variable.func1();
  }

  public void func1() {
    System.out.println("global = " + global);
  }
}
{% endhighlight %}
```
0
```

## 區域變數
在方法()之內宣告的變數為區域變數，生命周期跟著方法()執行完畢，區域變數會被記憶體回收。<br>
所謂的記憶體回收是指，告訴系統某一區塊的記憶體已經沒人使用，可以拿去存放其它變數。<br>

區域變數沒有預設值，使用前一定要指派值給區域變數。<br>
以下編譯失敗，因為要試圖輸出沒有值的區域變數。<br>
{% highlight java linenos %}
  public void func1() {
    int local;
    System.out.println(local);
  }
{% endhighlight %}