---
title: String Buffer
date: 2025-06-04
keywords: java, String Buffer
---
## 字串存放位置
String Buffer繼承AbstractStringBuilder。

字串存放位置是在AbstractStringBuilder中的value變數，不是final，byte\[\]陣列是物件，是存在Heap中。
{% highlight java linenos %}
abstract sealed class AbstractStringBuilder implements Appendable, CharSequence
    byte[] value;  // 字串存在這
}
{% endhighlight %}

## String Buffer與String

String的存放字串的位置value，是final的，所以更新字串，就要替換value指向的記憶體位址。

String Buffer存放字串的位置value，不是final，所以更新字串，是更新value的內容。

