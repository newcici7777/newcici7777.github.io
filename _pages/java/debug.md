---
title: debug
date: 2025-06-25
keywords: Java, debug
---
## 對JDK進行Debug
有時候要追到jdk原始碼裡面來debug，但要有以下的設定才能setp into JDK原始碼。

![img]({{site.imgurl}}/editor/debug1.png)

### 沒有force step into的按鈕
上面的設定有設的話，基本的step into(F7)就可以進到JDK原始碼。

如果還是沒辦法進去JDK原始碼，就要試著force step into。

以下步驟可以產生force step into按鈕，可以透過force step into進入JDK原始碼。

![img]({{site.imgurl}}/editor/debug2.png)

![img]({{site.imgurl}}/editor/debug3.png)

![img]({{site.imgurl}}/editor/debug4.png)

![img]({{site.imgurl}}/editor/debug5.png)

![img]({{site.imgurl}}/editor/debug6.png)

Force step into快速鍵。

macOS：Option + Shift + F7

Windows：Alt + Shift + F7

### 測試
請在Arrays.sort()這一行下中斷點，然後按step into的按鈕，或者按F7，檢查是否進入JDK原始碼。
{% highlight java linenos %}
public class Type {
  public static void main(String[] args) {
    int[] arr = {100, 20, 64, 1, 98};
    Arrays.sort(arr);  // 下中斷點
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/editor/debug7.png)