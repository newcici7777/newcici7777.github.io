---
title: 字串連結
date: 2024-07-29
keywords: c++, strcat 
---

## strcat
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

## strncat

{% highlight c++ linenos %}
char* myStrNCat(char* dest,const char* src,const size_t n) {
    //先把dest字串的長度先存下來
    size_t len = strlen(dest);
    memcpy(dest + len, src, n);
    *(dest + len + n) = 0;
    return dest;
}
{% endhighlight %}

函式只拷貝src字串中的n個，並在拷貝n個後加上\'\0\'。

以下是只拷貝2個字元的例子。

|dest字元|H|e|l|l|o|\0||||||
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|移動前|^|||||||||||
|移動+5||||||^||||||
|移動+5+2||||||||^||||
|拷貝src||||||W|o|\0||||

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