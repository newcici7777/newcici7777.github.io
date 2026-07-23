---
title: datetime
date: 2026-07-23
keywords: python, datetime
---
{% highlight python linenos %}
from datetime import datetime

# 年 月 日 時 分 秒
print(datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
{% endhighlight %}
```
2026-07-23 14:37:05
```