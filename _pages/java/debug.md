---
title: debug
date: 2025-06-25
keywords: Java, debug
---
## 對JDK進行Debug
有時候要追到jdk原始碼裡面來debug，但要有以下的設定才能setp into JDK原始碼。

IntelliJ -> Settings

![img]({{site.imgurl}}/editor/debug1.png)

把勾勾取消，代表可以進入jdk原始碼。

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
請在Arrays.sort()這一行設中斷點。
{% highlight java linenos %}
public class Type {
  public static void main(String[] args) {
    int[] arr = {100, 20, 64, 1, 98};
    Arrays.sort(arr);  // 設中斷點
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/editor/debug7.png)

然後滑鼠右鍵，按Debug。

![img]({{site.imgurl}}/editor/debug8.png)

然後按step into的按鈕，或者按F7，檢查是否進入JDK原始碼。

![img]({{site.imgurl}}/editor/debug9.png)

若要離開方法，按step out。

執行下一行，setp over。

## console
請在`System.out.println(arr[i]);`設中斷點。
{% highlight java linenos %}
int[] arr = {100, 20, 64, 1, 98};
Arrays.sort(arr);
for (int i = 0; i < arr.length; i++) {
  System.out.println(arr[i]);
}
{% endhighlight %}

然後Debug。

執行setp over，移到下一行。

console的部分有黃色圖示，代表有資料在console顯示。

![img]({{site.imgurl}}/editor/debug10.png)

切換至console。

![img]({{site.imgurl}}/editor/debug11.png)

## 顯示正確集合大小
要Debug ArrayList查看容量變化大小，要把以下勾勾取消。

![img]({{site.imgurl}}/editor/debug12.png)

![img]({{site.imgurl}}/editor/debug13.png)