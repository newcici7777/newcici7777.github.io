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