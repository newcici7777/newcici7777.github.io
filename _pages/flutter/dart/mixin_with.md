---
title: Mixin with
date: 2026-04-11
keywords: flutter, Mixin, with
---
Mixin with 很像Java 的「功能」介面 Interface。<br>

功能語法:
```
mixin 功能 {
	void 功能函式() {
		// 功能程式碼
	}
}
```

擁有功能的語法:
```
class 類別 with 功能 {}
```

客制化(覆寫)功能函式語法:
```
class 類別 with 功能 {
  @override
  void 功能函式() {
    // 要客制化的函式內容
  }
}
```

假設有一個「通用」飛的功能。
{% highlight dart linenos %}
mixin Fly {
  void fly() {
    print('fly');
  }
}
{% endhighlight %}

鴨子跟飛機都要有飛的功能。
{% highlight dart linenos %}
class Duck with Fly {}

class Airplane with Fly {}
{% endhighlight %}

執行鴨子飛、飛機飛的「通用」飛的功能。<br>
{% highlight dart linenos %}
void main() {
  Duck duck = Duck();
  duck.fly();
  Airplane airplane = Airplane();
  airplane.fly();
}
{% endhighlight %}
```
fly
fly
```

鴨子跟飛機各自寫自己的飛的方法。
{% highlight dart linenos %}
class Duck with Fly {
  @override
  void fly() {
    print('duck fly');
  }
}

class Airplane with Fly {
  @override
  void fly() {
    print('airplane fly');
  }
}
{% endhighlight %}

重新執行的結果就是
```
duck fly
airplane fly
```