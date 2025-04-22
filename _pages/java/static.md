---
title: Static
date: 2025-04-19
keywords: Java, static
---
Prerequisites:

- [記憶體配置][1]

## 靜態變數
在類別中，除了有成員變數之外，還有一個靜態變數。

什麼是靜態變數？靜態變數可以當作共享資源，所有物件都可以共享這個資源。

使用方法:  
```
類別名.靜態變數
```

## 靜態方法

靜態方法使用方法有下列二種

使用方法:  
```
類別名.靜態方法()
```

## 靜態方法不能存取物件成員變數與成員方法

- [c++靜態類別變數與函式][2]

靜態方法不能存取物件變數，只能存取靜態變數，因為類別在建立物件之前，物件的成員變數與成員方法都還沒有記憶體位址，但靜態變數與方法在程式碼(byte code)載入到code segment時，就已經在記憶體中bss segment或data segment建立靜態變數與靜態方法的記憶體位址。

## 靜態內部類別
{% highlight java linenos %}
public class Outer {
    public static class PublicStaticInner {}
    private static class PrivateStaticInner {}
}
{% endhighlight %}

[1]: {% link _pages/c/dynamicMemory/memoryLayout.md %}
[2]: {% link _pages/c/class/static_class.md %}