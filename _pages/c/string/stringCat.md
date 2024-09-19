---
title: 字串連結
date: 2024-07-29
keywords: c++, strcat 
---

Prerequisites:

- [記憶體間隔計算][1]

## strcat 字串連結
{% highlight c++ linenos %}
    memcpy(dest + strlen(dest), src, strlen(src) + 1);
{% endhighlight %}

假設dest字串為\"Hello\0\"，strlen(dest)回傳5，不包含\'\0\'。
dest + 5的意思是，指標往右移動5格。

|dest字元|H|e|l|l|o|\0|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|移動前|^||||||
|移動+5格||||||^|

此時dest指標的位置在\'\0\'。

假設src字串為\"World\0\"，
strlen(src) + 1是指要拷貝src全部字串， + 1是包含src的\'\0\'也一併拷貝到dest。

從dest所在的指標位址開始拷貝src的字元。

|dest字元|H|e|l|l|o|\0||||||
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|移動前|^|||||||||||
|移動+5格||||||^||||||
|拷貝src||||||W|o|r|l|d|\0|

{% highlight c++ linenos %}
//參數2因為是常數字串，所以型態為const char*
char* myStrCat(char* dest,const char* src) {
	//拷貝src字串
    memcpy(dest + strlen(dest), src, strlen(src) + 1);
    //返回dest開始位址
    return dest;
}
int main() {
    char str[20];
    //清空陣列記憶體中的值
    memset(str, 0, sizeof(str));
    strcpy(str, "Hello");
    myStrCat(str,"World");
    cout << "str = " << str << endl;
    return 0;
}
{% endhighlight %}

```
str = HelloWorld
```

## strncat 拷貝n個字元

```
memcpy(dest + len, src, n);
```
參數1 = dest + len 將位址移動到要拷貝的起始位址

參數2 = src 從src[0]位址開始拷貝字元

參數3 = n 拷貝幾個字元

目的字串 = Hello

來源字串 = World

拷貝個數 = 2

將來源字串Wo(二個字元)，接到Hello字串後面。

### 將目的字串位址移到最後面

|dest字元|H|e|l|l|o|\0||||||
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|移動前|^|||||||||||
|移動+5||||||^||||||

### 將Wo連到目的字串(Hello)位址最後面

|dest字元|H|e|l|l|o|\0||||||
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|拷貝src||||||W|o|||||


### 連結完來源字串Wo後，在最後面加上空字元

```
*(dest + len + n) = 0;
```

|dest字元|H|e|l|l|o|\0||||||
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|加上空字元||||||W|o|\0||||

{% highlight c++ linenos %}
char* myStrNCat(char* dest,const char* src,const size_t n) {
    //先把dest字串的長度先存下來
    size_t len = strlen(dest);
    memcpy(dest + len, src, n);
    *(dest + len + n) = 0;//將目的字串與拷貝的字串最後面添加結尾字元0
    return dest;
}
{% endhighlight %}


完整程式碼

{% highlight c++ linenos %}
char* myStrNCat(char* dest,const char* src,const size_t n) {
    //先把dest字串的長度先存下來
    size_t len = strlen(dest);
    memcpy(dest + len, src, n);
    *(dest + len + n) = 0;
    return dest;
}
int main() {
    char str[20];
    //清空陣列記憶體中的值
    memset(str, 0, sizeof(str));
    strcpy(str, "Hello");
    myStrNCat(str,"World",2);
    cout << "str = " << str << endl;
    return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/dynamicMemory/memory_interval.md %}