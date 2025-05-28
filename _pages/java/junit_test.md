---
title: Junit Test
date: 2025-05-28
keywords: Java, Junit Test
---
把以下的文字複製貼上在程式碼import的位置，如下圖一樣，滑鼠移到junit的字上，會出現下圖的畫面，若沒出現，可以把滑鼠放在junit的字之間，按下alt + Enter，看是否有任何反應。
```
import org.junit.jupiter.api.Test;
```

![img]({{site.imgurl}}/java/junit1.png)

![img]({{site.imgurl}}/java/junit2.png)

![img]({{site.imgurl}}/java/junit3.png)

![img]({{site.imgurl}}/java/junit4.png)

在方法上面寫上`@Test`，應該會出現如下圖中，2的部分多出一個執行按鈕。

![img]({{site.imgurl}}/java/junit5.png)

在方法的內容輸入
{% highlight java linenos %}
System.out.println("測試");
{% endhighlight %}

可以選擇Run或debug。

![img]({{site.imgurl}}/java/junit6.png)

會印出測試報告。

![img]({{site.imgurl}}/java/junit7.png)