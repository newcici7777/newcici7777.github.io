---
title: Future
date: 2026-04-11
keywords: flutter, dart, Future ,then, async
---
Future 有二種用法，一種是用then，一種是在函式中用async來使用Future。

## then
建立語法:
```
Future(() {
    return 回傳值;
});
```

成功:
```
then((value) {
  // 處理 回傳值 程式碼
});
```

失敗:
```
catchError((error) {
  // 處理error程式碼
});
```

成功範例:
{% highlight dart linenos %}
void main() {
  Future<String> f = Future(() {
    // 傳送字串
    return 'Hello world';
  });
  f.then((value) {
    // 輸出字串
    print(value);
  });
  f.catchError((error) {
    print(error);
  });
}
{% endhighlight %}
```
Hello world
```

失敗範例:
{% highlight dart linenos %}
void main() {
  Future<String> f = Future(() {
    throw Exception('my error');
  });
  f.then((value) {
    print(value);
  });
  f.catchError((error) {
    print(error);
  });
}
{% endhighlight %}
```
Exception: my error
```

## async
### 無接收傳回值語法
```
傳回值類型 函式名() async {
  try {
    await Future(() {
        return 傳回值;
    });
    等待上一步執行成功後，要處理的程式碼
  } catch (error) {
    處理error
  }
}
```
完整程式碼:
{% highlight dart linenos %}
void main() {
  test();
}

void test() async {
  try {
    await Future(
      () {
        return 'Hello world';
      },
    );
    print('success');
  } catch (error) {
    print(error);
  }
}
{% endhighlight %}

### 接收傳回值語法
```
傳回值類型 函式名() async {
  try {
    類型 result = await Future(
      () {
        return 傳回值;
      },
    );
    等待上一步執行成功後會輸出result
  } catch (error) {
    處理error
  }
}
```
完整程式碼:
{% highlight dart linenos %}
void main() {
  test();
}

void test() async {
  try {
    String result = await Future(
      () {
        return 'Hello world';
      },
    );
    print('success $result');
  } catch (error) {
    print(error);
  }
}
{% endhighlight %}

### 處理error
{% highlight dart linenos %}
void test() async {
  try {
    String result = await Future(
      () {
        throw Exception('my error');
      },
    );
    print('success $result');
  } catch (error) {
    print(error);
  }
}
{% endhighlight %}

### 增加等待時間
在Java中都有sleep()方法，暫時睡著。<br>

以下是dart的寫法。<br>
{% highlight dart linenos %}
void test() async {
  try {
    await Future.delayed(Duration(seconds: 3));
    print('success');
  } catch (error) {
    print(error);
  }
}
{% endhighlight %}

-----------------------


{% highlight dart linenos %}
onPressed: () async {
   currentOptions = await showSetUp(ctx, currentOptions, _localeNames);
},
{% endhighlight %}

這在 Dart / Flutter（以及大多數現代程式語言的非同步處理）中，await代表著「等待」的概念。

async 關鍵字：

當你在匿名函式加上 async 時，代表這個函式內部可以使用 await 來做非同步等待。

await showSetUp(...)：

當程式執行到這裡時，會呼叫 showSetUp 函式（也就是彈出下方的設定選單 BottomSheet）。

因為 showSetUp 裡面包含使用者在選單裡點擊、輸入、甚至放著不理直到關閉對話框的過程，這需要花費不確定的時間。

await 會讓程式在這裡「暫停等待」，直到使用者關閉對話框、並且 showSetUp 回傳更新後的 updatedOptions 物件為止。

賦值與後續執行：

當對話框關閉、資料回傳後，才會把結果指定給 currentOptions。

如果這個匿名函式後面還有其他程式碼，必須等到 await 這行完全跑完後，才會接著執行下一行。

簡單來說，它就像在說：「我要打開設定選單，請停在這邊等使用者調完、按關閉，拿到最新的設定結果之後，我才要往下繼續做事。」