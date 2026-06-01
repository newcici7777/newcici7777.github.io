---
title: sleep ctime
date: 2026-06-01
keywords: Python, sleep, ctime
---
把time的套件匯入
```
import time
```

sleep語法
```
sleep(秒數)
```
以下程式碼是睡10秒。<br>

現在時間ctime()
```
time.ctime()
```

{% highlight python linenos %}
import time
print(f"開始等待 {time.ctime()}")
time.sleep(10)
print(f"結束等待 {time.ctime()}")
{% endhighlight %}
```
開始等待 Mon Jun  1 13:08:17 2026
結束等待 Mon Jun  1 13:08:27 2026
```