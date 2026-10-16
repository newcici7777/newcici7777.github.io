---
title: get
date: 2026-10-05
keywords: flutter, get
---

1. 什麼是 Getter (get)？
Getter 讓你可以像存取一般變數一樣去取得資料，但背後其實執行了一段程式碼（或回傳某個運算結果）。

例如，在一個普通的類別中，你可以這樣寫：
{% highlight dart linenos %}
class User {
  String firstName = 'John';
  String lastName = 'Doe';

  // 這是一個 getter，用來組合全名
  String get fullName => '$firstName $lastName';
}

void main() {
  var user = User();
  print(user.fullName); // 輸出 'John Doe'（注意：呼叫時不需要加括號像函式一樣）
}
{% endhighlight %}


-----------------------

2. 為什麼後面只有分號 ;？
當 get 後面直接接分號（沒有大括號 {} 和回傳值），它就是一個抽象 Getter。

情境：通常用於定義狀態管理（例如 BLoC、Cubit 或 ViewModel）或抽象介面。

含意：「我這個類別不決定 error 裡面要裝什麼，但我規定所有繼承我的子類別，都必須實作這個 error 屬性，讓外界可以讀取錯誤訊息。」

子類別實作範例：

```
// 抽象類別
abstract class FormState {
  String? get error; // 抽象 getter
}

// 子類別必須實作它
class LoginFormState extends FormState {
  @override
  String? get error => '密碼錯誤'; // 具體實作
}
```

get：定義一個唯讀的屬性，透過運算或回傳內部狀態來提供資料。

String?：型態為可為空的字串（Nullable String）。

結尾的 ;：代表這是抽象定義，規定子類別一定要實作這個屬性。