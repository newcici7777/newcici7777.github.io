---
title: 函式
date: 2026-03-30
keywords: flutter, dart, function
---
函式宣告的方式跟 Java 一樣，在此省略不說。<br>

## 函式只有一行 `=>`
若函式內的程式碼只有一行，使用`=>`。<br>
{% highlight dart linenos %}
void main() {
  var res = sum(1, 2);
  print(res);
}

int sum(int a, int b) => a + b;
{% endhighlight %}
```
3
```

## 傳回值類型
### 沒有傳回值類型
dart 會自動推導類型。<br>
{% highlight dart linenos %}
void main() {
  var res = func2();
  print(res.runtimeType);
  print(res.startsWith("H"));
  print(res);
}

func2() {
  return "Hello World";
}
{% endhighlight %}
```
String
true
Hello World
```

### 有傳回值類型
void 也是類型的一種。<br>
{% highlight dart linenos %}
void func1() {
  print("func1");
}

String func3() {
  return "func3";
}
{% endhighlight %}

## 函式參數
- 必要參數 (required positional parameters)
- 可選位置參數 `[]` (optional positional parameters)
- 可選命名參數 `{}` (Named parameters)

### 必要參數
呼叫函式時，參數一定要傳遞，並且按照位置傳遞。
{% highlight dart linenos %}
void main() {
  func1(100);
}

void func1(int param1) {
  print("param1: $param1");
}
{% endhighlight %}

### 可選位置參數
可以不傳參數，也可以只傳一個，但要按照順序傳。

參數語法:<br>
```
[參數1?, 參數2?]
[參數1 = 預設值, 參數2 = 預設值]
```

若參數沒給預設值，參數後面要有?問號，代表參數是nullable類型(可以是null)，預設值也為null。<br>
若參數有給預設值，參數後面不用有?問號。<br>

#### 可選位置參數 null
若參數沒給預設值，參數後面要有?問號。<br>
{% highlight dart linenos %}
void main() {
  // 可以不傳參數
  func3();
  // 可以只傳一個
  func3("a");
  func3("a", 10);
}

void func3([String? a, int? b]) {
  print("a: $a, b: $b");
}
{% endhighlight %}
```
a: null, b: null
a: a, b: null
a: a, b: 10
```

#### 可選位置參數 non-null
若參數有給預設值，參數後面不用有?問號。<br>
{% highlight dart linenos %}
void main() {
  func3();
  func3("a");
  func3("a", 10);
}

void func3([String a = "Hi", int b = 0]) {
  print("a: $a, b: $b");
}
{% endhighlight %}
```
a: Hi, b: 0
a: a, b: 0
a: a, b: 10
```

#### 必要參數 + 可選位置參數
如果有「必要參數」，「可選位置參數」放在「必要參數」的後面。<br>
{% highlight dart linenos %}
void main() {
  func3("Hello");
  func3("Hello", "a");
  func3("Hello", "a", 10);
}

void func3(String param1, [String? a, int? b]) {
  print("param1: $param1, a: $a, b: $b");
}
{% endhighlight %}
```
param1: Hello, a: null, b: null
param1: Hello, a: a, b: null
param1: Hello, a: a, b: 10
```

### 可選命名參數
可以不傳參數，也可以只傳一個，傳遞時要加上參數名，不用按照順序傳。

使用大括號包住參數名，語法:<br>
```
{類型 參數名1?, 類型 參數名2?}
{類型 參數名1 = 預設值, 類型 參數名2 = 預設值}
```

函式呼叫語法，使用冒號:分隔。<br>
```
函式(參數名1: 參數, 參數名2: 參數)
```

傳遞時要加上參數名，不用按照順序傳。<br>
{% highlight dart linenos %}
void main() {
  func4();
  func4(name: "Cici");
  func4(age: 18, name: "Cici");
}

void func4({String? name, int? age}) {
  print("name: $name, age: $age");
}
{% endhighlight %}
```
name: null, age: null
name: Cici, age: null
name: Cici, age: 18
```

{% highlight dart linenos %}
void main() {
  func4();
  func4(name: "Cici");
  func4(age: 18, name: "Cici");
}

void func4({String name = "none", int age = 0}) {
  print("name: $name, age: $age");
}
{% endhighlight %}
```
name: none, age: 0
name: Cici, age: 0
name: Cici, age: 18
```

#### 必要參數 + 可選命名參數
如果有「必要參數」，「可選命名參數」放在「必要參數」的後面。<br>

不可以「必要參數」\+ 「可選命名參數」 \+ 「可選位置參數」。<br> 

## 匿名函式
所謂的匿名函式，就是把「程式邏輯」作為參數，傳遞給函式。<br>

### 匿名函式類型 Function
參數類型為Function。
{% highlight dart linenos %}
void func1(Function callback) {
  callback();
}
{% endhighlight %}

### 無參數匿名函式語法
```
() {
    程式邏輯
}
```
把`print("Hello");` 的程式邏輯，傳給參數。<br>
{% highlight dart linenos %}
void main() {
  func1(() {
    print("Hello");
  });
}

void func1(Function callback) {
  callback();
}
{% endhighlight %}

### 有參數匿名函式語法
{% highlight dart linenos %}
void main() {
  func1((String? param1) {
    print("Hello $param1");
  });
}

void func1(Function callback) {
  callback("Bill");
}
{% endhighlight %}
```
Hello Bill
```

### 匿名函式指派給變數
語法
```
Function 變數名 = () {
	程式碼
}
```

{% highlight dart linenos %}
void main() {
  func1(function1, "Bill");
}

Function function1 = (String? param1) {
  print("Hello $param1");
};

void func1(Function callback, String? name) {
  callback(name);
}
{% endhighlight %}
```
Hello Bill
```