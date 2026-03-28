---
title: Nullable type
date: 2026-03-28
keywords: flutter, dart, Nullable Type, Non-nullable Type
---
Nullable Type 又稱為 可以儲存null的類型。<br>
在dart中，變數分為可以儲存 null ，跟不可以儲存 null。<br>

## Non-nullable Type 不可以儲存 null 類型
以下的程式碼，試圖讓 str 變數儲存 null，會產生編譯錯誤(語法錯誤)。<br>
因為以下是Non-nullable(不可為null)的類型。<br>
{% highlight dart linenos %}
void main() {
  String str = null;
}
{% endhighlight %}

以下的程式碼也編譯錯誤，因為str是不可為null的類型，不能指派 null 給 str變數。<br>
{% highlight dart linenos %}
void main() {
  String str = "Hello";
  str = null;
}
{% endhighlight %}

## Nullable Type 可以儲存 null 類型
類型旁邊加上問號?，就會變成Nullable Type(可以儲存 null 類型)。
```
類型? 變數 = null;
```
{% highlight dart linenos %}
void main() {
  String? str = null;
}
{% endhighlight %}

## `?.`存取 Nullable Type 類型
以下程式碼要對null 進行 方法 或 屬性 的操作，IDE會產生編譯錯誤，無法編譯。<br>
因為null 物件，本身就沒有.length 的屬性。<br>
{% highlight dart linenos %}
void main() {
  String? str = null;
  print(str.length);
}
{% endhighlight %}

使用`?.` 可以安全的呼叫方法或屬性，若值是 null，就會傳回null，不會執行取得屬性、呼叫方法的操作。<br>
語法:<br>
```
類型? 變數名 = null;
```

{% highlight dart linenos %}
void main() {
  String? str = null;
  print(str?.length);
}
{% endhighlight %}
```
null
```

## `!` 可為Null類型 假設為 不可為Null類型。
### 範例1
如果變數是Non-Nullable不可以為null的類型，但指派的變數是Nullable 可以為null的類型。<br>

以下執行時會有錯誤，因為不可以把「可以null」的類型，指派給「不可以null」的類型。<br>
因為程式不知道可以null的類型，裡面的值是null。<br>
如果裡面的值是null，就不能指派給「不可以null」的類型。<br>
{% highlight dart linenos %}
void main() {
  // Non-Nullable
  String str1 = "";
  // Nullable
  String? str2 = "Hello";
  // 可以null類型 指派給 不可以null類型
  str1 = str2;
  print(str1);
}
{% endhighlight %}
```
Unhandled exception:
Null check operator used on a null value
```

使用!驚嘆號把「可以Null」的類型，假設成「不可以Null」的類型。<br>
但是會出現警告，但仍是可以執行。<br>
{% highlight dart linenos %}
  String str1 = "";
  // Nullable
  String? str2 = "Hello";
  // 可以null類型 指派給 不可以null類型
  str1 = str2!;
  print(str1);
{% endhighlight %}
```
01.dart:7:10: Warning: Operand of null-aware operation '!' has type 'String' which excludes null.
  str1 = str2!;
         ^
Hello
```

但如果值是null，執行時就會產生錯誤。<br>
{% highlight dart linenos %}
void main() {
  String str1 = "";
  String? str2 = null;
  str1 = str2!;
  print(str1);
}
{% endhighlight %}
```
Unhandled exception:
Null check operator used on a null value
```

### 範例2
如果要對「可以Null」的類型使用屬性或方法。<br>
以下會編譯錯誤。<br>
{% highlight dart linenos %}
  String? str1 = null;
  print(str1.length);
{% endhighlight %}

使用!驚嘆號，不管變數的內容是Null或不是Null，都假設它為「不可以Null」類型。<br>
{% highlight dart linenos %}
void main() {
  String? str1 = null;
  print(str1!.length);
}
{% endhighlight %}

### 範例3
func1()函式參數str為「不可以null」的類型，str2為「可以null」的類型。<br>
把str2可以null的類型，傳入參數為「不可以null」的類型，執行時會產生錯誤。<br>
{% highlight dart linenos %}
void main() {
  String? str2 = "Hello";
  func1(str2);
}

void func1(String str) {
  print(str);
}
{% endhighlight %}
```
Unhandled exception:
Null check operator used on a null value
```

使用驚嘆號，假設str2為「不可為Null」類型。<br>
{% highlight dart linenos %}
void main() {
  String? str2 = "Hello";
  func1(str2!);
}

void func1(String str) {
  print(str);
}
{% endhighlight %}
```
01.dart:3:9: Warning: Operand of null-aware operation '!' has type 'String' which excludes null.
  func1(str2!);
        ^
Hello
```

## ?? null 就傳回預設值
如果變數是Null，就傳回預設值。<br>
```
變數?? 預設值
``` 

若 str1 為 null，就把`""` 指派給str1。<br>
{% highlight dart linenos %}
void main() {
  String? str1 = null;
  String str2 = str1 ?? "";
  print(str2);
}
{% endhighlight %}
```

```

## 使用 if 判斷不是 null
若 if 判斷不為 null ，就可以把str1 指派給 str2。<br>
注意！此處變數不用!驚嘆號，就可以直接把str2指派給str1。<br>
因為dart 認定你已經處理null，就不用強制你加上驚嘆號!。<br>
{% highlight dart linenos %}
void main() {
  String? str1 = null;
  String str2 = "";
  if (str1 != null) {
    str2 = str1;
  }
  print(str2);
}
{% endhighlight %}

注意！以下程式碼，變數後面不用!驚嘆號，也不用?問號，就可以直接存取.length屬性。<br>
因為dart 認定你已經處理null，就不用強制你加上驚嘆號!與?問號。<br>
{% highlight dart linenos %}
  String? str1 = null;
  if (str1 != null) {
    print(str1.length);
  }
{% endhighlight %}