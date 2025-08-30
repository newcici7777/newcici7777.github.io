---
title: KMP
date: 2025-08-29
keywords: java, KMP
---
## prefix與suffix
prefix 中文為前綴，英文字根。<br>
suffix 中文為後綴，英文字尾。<br>

想像一下，學習英文，有一些字首字根，就可以猜出字母的意思，比如pre有事前、預備。<br>
英文字尾為tion為名詞。<br>

### 前綴
但這邊的前綴是以下這樣的，不包含字尾。<br>
字串 = abab<br>
a<br>
<span class="markline">ab</span><br>
aba<br>

### 後綴
後綴是以下這樣的，不包含字首。<br>
字串 = abab<br>
aba<u>b</u> → b<br>
ab<u>ab</u> → <span class="markline">ab</span><br>
a<u>bab</u> →  bab<br>

### 共同前綴後綴
共同前綴後綴，意思是前綴與後綴中都擁有相同字串。
abab前綴與後綴都擁有相同的字串為:<span class="markline">ab</span>

## next陣列
a只有一個字母，前綴不包含字尾，後綴不包含字首。所以共同前後綴為0。<br>
ab，沒有共同前後綴，所以為0。<br>

aba:<br>
前綴:a ab<br>
後綴:a ba<br>
共同前後綴為a<br>
a字串長度為1，共同前後綴長度為1。<br>

abab:<br>
前綴:a ab aba<br>
後綴:b ab bab<br>
共同「最長」前後綴為ab，字串長度為2。<br>

|pattern字串 |a|b|a|b|
|共同前後綴長度|0|0|1|2|

### next陣列程式碼步驟
預設只有一個字元，不會有共同前後綴。
所以`next[0] = 0`

j是指向「前」綴，i是指向「後」綴。<br>
ab，j指向a，i指向b，二個字母不相同，沒有共同前後綴，把j指向的索引0，塞入next陣列。<br>
i\+\+，往前移動i=2。<br>
```
next[i=1] = 0
```
![img]({{site.imgurl}}/java_datastruct/kmp_p1.png)<br>

j指向a，i指向a，二個字母相同，j\+\+，j往前移動，j=1。<br>
![img]({{site.imgurl}}/java_datastruct/kmp_p2.png)<br>

j指向索引1，把j指向的索引1，塞入next陣列。<br>
```
next[i=2] = 1
```
![img]({{site.imgurl}}/java_datastruct/kmp_p3.png)<br>

i\+\+，往前移動i=3。<br>
![img]({{site.imgurl}}/java_datastruct/kmp_p4.png)<br>

j指向b，i指向b，二個字母相同，j\+\+，j往前移動，j=2。<br>
![img]({{site.imgurl}}/java_datastruct/kmp_p5.png)<br>

j指向索引2，把j指向的索引2，塞入next陣列。<br>
```
next[i=3] = 2
```
![img]({{site.imgurl}}/java_datastruct/kmp_p6.png)<br>

i\+\+，往前移動i=4。<br>
j指向a，i指向c，二個字母不相同。<br>
![img]({{site.imgurl}}/java_datastruct/kmp_p7.png)<br>

j = 2，套用以下的公式，找出`next[1] = 0`，j移動到索引0的位置。
```
j = next[j - 1]
j = next[2 - 1]
```
![img]({{site.imgurl}}/java_datastruct/kmp_p8.png)<br>

j指向a，i指向c，二個字母仍是不相同，把j指向的索引0，塞入next陣列。
```
next[i=4] = 0
```
![img]({{site.imgurl}}/java_datastruct/kmp_p9.png)<br>

## 多次回推的next陣列範例
pattern字串 = abababca

目前i指向索引7，j指向5。



{% highlight java linenos %}
public class Kmp {
  public static void main(String[] args) {
    String str = "aabaa";
    String pattern = "aaa";
    int[] next = getNext(pattern);
    System.out.println(Arrays.toString(next));
    int i = 0, j = 0 ;
    int findIndex = -1;
    for(;i < str.length(); i++) {
      while (j > 0 && str.charAt(i) != pattern.charAt(j)) {
        j = next[j-1];
      }
      if (str.charAt(i) == pattern.charAt(j)) {
        j++;
      }
      if (j == pattern.length()) {
        findIndex = i - j + 1;
        break;
      }
    }
    if (findIndex != -1) {
      System.out.println("find index = " + findIndex);
    } else {
      System.out.println("can not find index.");
    }
  }
  public static int[] getNext(String pattern) {
    int[] next = new int[pattern.length()];
    int j = 0;
    next[0] = 0;
    for (int i = 1; i < pattern.length(); i++) {
      while (j > 0 && pattern.charAt(j) != pattern.charAt(i)) {
        j = next[j - 1];
      }
      if (pattern.charAt(i) == pattern.charAt(j)) {
        j++;
      }
      next[i] = j;
    }
    return next;
  }
}
{% endhighlight %}