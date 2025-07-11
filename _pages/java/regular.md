---
title: 規則表示式
date: 2025-07-10
keywords: Java, Regular expression
---
## 套件
```
import java.util.regex.Pattern;
```

## 跳脫字元
以下這些跳脫字元，使用時，前面要加上2個右斜`\\`，注意！不是一個。
```
.*+()$/\?[]{}^
```

在content的內容中，比對跳脫字元`.`。
{% highlight java linenos %}
String content = "abcdef.hijkl.";
String regStr = "\\.";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
.
.
```

## \[\]比對一個字母或數字
### \[\]或
被括號\[\]包住的字母或數字，假設是`[abc123]`，這個規則的意思是，找尋是否有a或b或c或1或2或3，任一個字母。<br>
<span class="markline">aaabbcc</span>def.hi<span class="markline">321</span>45<span class="markline">1</span>6<span class="markline">2</span>78jkl.<br>

{% highlight java linenos %}
String content = "aaabbccdef.hi3214516278jkl.";
String regStr = "[abc123]";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
a
a
a
b
b
c
c
3
2
1
1
2
```

### \[^\]相反
被括號\[\]包住的字母或數字，假設是`[^abc123]`，這個規則的意思是，找尋「不是」a或b或c或1或2或3，任一個字母。<br>

aaabbcc<span class="markline">def.hi</span>321<span class="markline">45</span>1<span class="markline">6</span>2<span class="markline">78jkl.</span>

{% highlight java linenos %}
String content = "aaabbccdef.hi3214516278jkl.";
String regStr = "[^abc123]";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
d
e
f
.
h
i
4
5
6
7
8
j
k
l
.
```
### \- 範圍
\-比對字母或數字的範圍。<br>
`[d-h1-3]`比對字母d到h，數字1到3，任一個字元。<br>

{% highlight java linenos %}
String content = "aaabbccdef.hi3214516278jkl.";
String regStr = "[d-h1-3]";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
d
e
f
h
3
2
1
1
2
```

### 表格說明

|\[\]或|說明|
|:---------|:---------------|
|`[abc]`|a或b或c，任一個字元。|
|`[^abc]`|「不是」a或b或c，其它任一個字元。|
|`[a-zA-Z]`|比對小寫大寫英文字母，任一個字元。|
|`[0-9]`|比對數字，任一個字元。|
|`[^0-9]`|比對「不是」數字，任一個字元。|


## 比對字串
比對「abc」字串，「三個一起」比對，跟前面的\[\]不同，\[\]方括號是比對裡面中任意一個字母或數字，而這裡是比對「abc」字串。
{% highlight java linenos %}
String content = "abcdefghijklmabcn";
String regStr = "abc";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
abc
abc
```
### 或\|
比對abc字串「或」ijk字串，比對二個字串，注意！這裡沒有方括號\[\]。
{% highlight java linenos %}
String content = "abcdefghijklmabcn";
String regStr = "abc|ijk";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}

## `\\d` `\\w`

### `\\d \\D`
`\\d`比對0-9的數字。<br>
`\\D`比對「非」0-9的數字<br>
{% highlight java linenos %}
String content = "abcde012fgh34ijklmabcn";
String regStr = "\\d";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
0
1
2
3
4
```

### `\\w \\W`
`\\w`比對任一英文字母、數字、底線。<br>
`\\W`比對任一「非」英文字母、數字、底線。<br>
跳脫字元，不屬於w。<br>

{% highlight java linenos %}
String content = "abc@$_ABC_012@~";
String regStr = "\\w";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
a
b
c
_
A
B
C
_
0
1
2
```

### 比對空白
{% highlight java linenos %}
String content = "abc abc abc";
String regStr = "\\s";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}

### .比對任一個字元
比對任一個不是`\n`之外的所有字元。
{% highlight java linenos %}
String content = "abc\nabc";
String regStr = ".";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println(matcher.group(0));
}
{% endhighlight %}
```
a
b
c
a
b
c
```
### 表格說明

|字元|說明|
|:---------|:---------------|
|.|比對任一個不是`\n`之外的所有字元。|
|`\\d`|比對任一個數字，`[0-9]`|
|`\\D`|比對任一個「非」數字，`[^0-9]`|
|`\\w`|比對任一個英文字母、數字、底線，`[a-zA-Z0-9_]`|
|`\\W`|比對任一個「非」英文字母、數字、底線，`[^a-zA-Z0-9_]`|
|`\\s`|比對任一個空白|
|`\\S`|比對任一個「非」空白，`[^\s]`|

## 重覆次數
### \+ 重覆至少1次至n次
以下範例比對a字母至少1次至n次
{% highlight java linenos %}
String content = "abcaabcaaabcaaaa";
String regStr = "a+";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:a
找到:aa
找到:aaa
找到:aaaa
```

### \* 重覆0次至n次
比對a為開頭的字母(重覆至少1次至n次)，後面可以有b也可以沒有b(重覆至少0次至n次)。
{% highlight java linenos %}
String content = "abaabbcac";
String regStr = "a+b*";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:ab
找到:aabb
找到:a
```

### ? 重覆0次至1次
比對a為開頭的字母(重覆至少1次至n次)，後面只能有「一個b」也可以沒有b(重覆至少0次至1次)。
{% highlight java linenos %}
String content = "abaabbcac";
String regStr = "a+b?";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:ab
找到:aab
找到:a
```

比對a開頭，後面只能有「一個1」也可以沒有1。
{% highlight java linenos %}
String content = "a11111111aaa";
String regStr = "a1?";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:a1
找到:a
找到:a
找到:a
```
### \{n\} 自訂重覆次數
以下程式碼，比對b重覆2次，等同bb<br>
其它還有\{n,m\}，自訂重覆至少n次至m次。<br>
\{n,\}，自訂重覆至少n次至無限次。<br>
{% highlight java linenos %}
String content = "abaabbcacbbb";
String regStr = "b{2}";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:bb
找到:bb
```

## 組合與次數
### \{n\}
之前\[\]代表「或」的意思，此處搭配次數，代表組合的意思。<br>
`[ab]{2}`代表由ab2個字母，任意組合成字串，字串長度為2個字母，可以是aa、ab、bb、ba。<br>
{% highlight java linenos %}
String content = "abaabbcacbbba";
String regStr = "[ab]{2}";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:ab
找到:aa
找到:bb
找到:bb
找到:ba
```
### \{n,m\}
比對abcd四個字母，任意組合成字串，字串長度至少2至3。
{% highlight java linenos %}
String content = "abfagbbcacebbba";
String regStr = "[abcd]{2,3}";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:ab
找到:bbc
找到:ac
找到:bbb
```

## `\\d \\w`與重覆次數
`\\d`比對任一個數字，等同`[0-9]`。<br>
`\\d`搭配次數，代表至少重覆幾次。<br>
以下程式碼比對數字0-9，0-9任意數字重覆次數為最少為2次，最多為3次。<br>
{% highlight java linenos %}
String content = "ab01fag2356bbc55555acebbba";
String regStr = "\\d{2,3}";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:01
找到:235
找到:555
找到:55
```

## 貪婪比對
優先比對次數多的。<br>
以下程式碼，有6個a，結果不是aaa、aaa，而是只比對到aaaa，4個a。<br>
{% highlight java linenos %}
String content = "aaaaaa";
String regStr = "a{3,4}";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:aaaa
```

以下程式碼，有8個1，結果不是1111、1111，而是只比對到11111，5個1。<br>
優先比對次數多的。
{% highlight java linenos %}
String content = "11111111";
String regStr = "1{4,5}";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:11111
```

優先比對次數多的。以下程式碼使用\+，1至多，盡可能比對最多數字。
{% highlight java linenos %}
String content = "11111111";
String regStr = "1+";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:11111111
```

## 位置字元

|字元|說明|
|^|開頭|
|\$|結尾|
|`\\b`|空格前的「結尾」或字串「結尾」|
|`\\B`|空格前的「開頭」或字串「開頭」|

### ^開頭
`^[abc][0-9]+`字串只能以「一個」a數字、b數字、c數字開頭。若是aa11，也不行，因為有2個a。
{% highlight java linenos %}
String content = "a11bb2335ccc1";
String regStr = "^[abc][0-9]+";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
a11
```

### \$結尾
字串只能以「一個」a數字、b數字、c數字開頭，一個跳脫字元\-結尾，跳脫字元在java中要用二個右斜線`\\跳脫字元`。
{% highlight java linenos %}
String content = "a1122-";
String regStr = "^[abc][0-9]+\\-$";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:a1122-
```

### 空格前結尾 字串結尾
以下程式碼，abc是比對條件。<br>
空格前結尾，意思是「ingabc 」字串，空格之前，結尾是不是「abc」三個字？<br>
字串結尾，意思是「abcingabc」字串，字串的結尾是不是「abc」三個字？<br>
ing<span class="markline">abc</span> ing<span class="markline">abc</span> <span class="markline">abc</span>
{% highlight java linenos %}
String content = "ingabc ingabc abc";
String regStr = "abc\\b";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:abc
找到:abc
找到:abc
```

### 空格後開頭 字串開頭
以下程式碼，abc是比對條件。<br>
空格後開頭，意思是「 abcing」字串，空格之後，開頭是不是「abc」三個字？<br>
字串開頭，意思是「abcing」字串，字串的開頭是不是「abc」三個字？<br>
<span class="markline">abc</span>ing <span class="markline">abc</span>ing abc
{% highlight java linenos %}
String content = "abcing abcing abc";
String regStr = "abc\\B";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到:" + matcher.group(0));
}
{% endhighlight %}
```
找到:abc
找到:abc
```

## 分組
### group\(0\)
group(0)記錄的是比對到字串的開始索引與結束索引。<br>
以下程式碼，規則是4個數字，相等於`\\d{4}`或`[0-9]{4}`。
{% highlight java linenos %}
String content = "2025";
String regStr = "\\d\\d\\d\\d";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("group0:" + matcher.group(0));
}
{% endhighlight %}
```
group0:2025
```

1.先使用中斷點，請像下圖中一樣，設定中斷點，並且執行debug。<br>
![img]({{site.imgurl}}/java/reg_debug1.png) <br>

2.把groups打開，發現全部都是-1。<br>
![img]({{site.imgurl}}/java/reg_debug2.png)<br>

3.按「step over」，F8，逐步執行。<br>
發現groups陣列索引`[0]`為0，是比對到字串<span class="markline">2</span>025的開始索引。<br>
發現groups陣列索引`[1]`為4，是比對到字串202<span class="markline">5</span>的結束索引。<br>
![img]({{site.imgurl}}/java/reg_debug3.png)<br>

#### matcher.group()原始碼
{% highlight java linenos %}
public String group(int group) {
    checkMatch();
    checkGroup(group);
    if ((groups[group*2] == -1) || (groups[group*2+1] == -1))
        return null;
    return getSubSequence(groups[group * 2], groups[group * 2 + 1]).toString();
}
{% endhighlight %}

以下的程式碼`groups[group * 2]`代表開始索引，`groups[group * 2 + 1]`代表結束索引，把group = 0代入，發現會取得索引`groups[0]`與`groups[1]`的字串。
{% highlight java linenos %}
getSubSequence(groups[group * 2], groups[group * 2 + 1])
{% endhighlight %}

### group\(1\)與group\(2\)
使用圓括號\(\)進行分組，分組序號是由1開始，注意！不是0開始。<br>
```
   1    2
(\d\d)(\d\d)
```

對以下程式碼，`while (matcher.find())`加上中斷點，跟之前步驟一樣。
{% highlight java linenos %}
String content = "2025";
String regStr = "(\\d\\d)(\\d\\d)";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("group0:" + matcher.group(0));
  System.out.println("group1:" + matcher.group(1));
  System.out.println("group2:" + matcher.group(2));
}
{% endhighlight %}
```
group0:2025
group1:20
group2:25
```

![img]({{site.imgurl}}/java/reg_debug4.png) <br>

group0:<br>
`groups[0] = 0`，是比對到字串<span class="markline">2</span>025的開始索引。<br>
`groups[1] = 4`，是比對到字串202<span class="markline">5</span>的結束索引。<br>

group1:<br>
`groups[2] = 0`，是比對到字串<span class="markline">2</span>025的開始索引。<br>
`groups[3] = 2`，是比對到字串2<span class="markline">0</span>25的結束索引。<br>

group2:<br>
`groups[4] = 2`，是比對到字串20<span class="markline">2</span>5的開始索引。<br>
`groups[5] = 4`，是比對到字串202<span class="markline">5</span>的結束索引。<br>

## 不是分組
以下的\(\)圓括號不是分組，不能使用`matcher.group(1)`。

### `(?:規則)`
`(?:規則)`跟「或\|」一樣。<br>
字串<span class="markline">win98</span>,<span class="markline">winxp</span>,win2000<br>
比對開頭為win，後面可以是98或xp的字串，等同`win98|winxp`。<br>
{% highlight java linenos %}
String content = "win98,winxp,win2000";
String regStr = "win(?:98|xp)";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到" + matcher.group(0));
}
{% endhighlight %}
```
找到win98
找到winxp
```
### `(?=規則)`
`win(?=98|xp)`<br>
字串<span class="markline">win</span>98,<span class="markline">win</span>xp,win2000<br>
比對開頭為win，後面是98或xp中的「win」。<br>
{% highlight java linenos %}
String content = "win98,winxp,win2000";
String regStr = "win(?=98|xp)";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到" + matcher.group(0));
}
{% endhighlight %}
```
找到win
找到win
```

### `(?!規則)`
`win(?!98|xp)`<br>
!代表相反，也就是「不是」的意思。<br>
字串win98,winxp,win<span class="markline">2000</span><br>
比對開頭為win，但後面「不是」98或xp中的「win」。<br>
{% highlight java linenos %}
String content = "win98,winxp,win2000";
String regStr = "win(?!98|xp)";
Pattern pattern = Pattern.compile(regStr);
Matcher matcher = pattern.matcher(content);
while (matcher.find()) {
  System.out.println("找到" + matcher.group(0));
}
{% endhighlight %}
```
找到win
```


[1]: {% link _pages/java/string_utils.md %}
