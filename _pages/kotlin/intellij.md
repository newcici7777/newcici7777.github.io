---
title: Intellj快速鍵
date: 2025-05-21
keywords: kotlin, Intellj快速鍵
---
## Decompile轉碼後的java
按2次shift鍵，輸入show kotlin bytecode 
![img]({{site.imgurl}}/editor/bytecode.png)

按下「Decompile」按鈕，可以看到轉碼過後的java程式碼。
![img]({{site.imgurl}}/editor/bytecode2.png)

## 全部快速鍵
win: ctrl \+ j
mac: cmd \+ j

## 變數.屬性.sout
s1.length.sout
{% highlight kotlin linenos %}
var s1 = "871"
// 輸入s1.length.sout
{% endhighlight %}

產生println(s1.length)
{% highlight kotlin linenos %}
var s1 = "871"
println(s1.length)
{% endhighlight %}

## 集合變數.for
IntelliJ快速鍵: I
IntelliJ快速鍵: 集合變數.for
{% highlight kotlin linenos %}
var s = "asd"
// s.for，就會有迴圈的指令
for (c in s) {
    println(c)
}
{% endhighlight %}

## iterator迴圈
itit

## try-catch
先「選取」可能會發生錯誤的程式碼。

按下快速鍵

Win: ctrl + alt + t

Mac: cmd + alt + t

選取你要的try...catch，我都是選6。

## 建立module
![img]({{site.imgurl}}/editor/module.png)

## sdk 設定
![img]({{site.imgurl}}/editor/sdk1.png)

![img]({{site.imgurl}}/editor/sdk2.png)

![img]({{site.imgurl}}/editor/sdk3.png)

## 快速進入類別
對著類別名，按下快速鍵。

Win: ctrl + b

Mac: cmd + b

## Structure
![img]({{site.imgurl}}/editor/struture1.png)

![img]({{site.imgurl}}/editor/struture2.png)

## 複製的時候不要把package一起複製上
![img]({{site.imgurl}}/editor/auto_import.png)