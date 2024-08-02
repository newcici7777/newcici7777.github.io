---
title: 字串拷貝
date: 2024-07-29
keywords: c++, 
---

## 陣列法

{% highlight c++ linenos %}
//參數2因為是常數字串，所以型態為const char*
char* myStrCpy(char* dest,const char* src) {
    //i為dest陣列的索引
    //j為src陣列的索引
    int i = 0, j = 0;
    // 如果來源字串不為0就進入迴圈
    while(src[j]) {
        //將src[j]的值給dest[i]，然後i++,j++
        dest[i++] = src[j++];
    }
    //將dest結尾放上空字元
    dest[i] = 0;
    return dest;
}
int main() {
    char name[20];
    //清空陣列記憶體中的值
    memset(name, 0, sizeof(name));
    myStrCpy(name, "Bill");
    cout << "name = " << name << endl;
    return 0;
}
{% endhighlight %}

```
name = Bill
```

## 指標法

{% highlight c++ linenos %}
//參數2因為是常數字串，所以型態為const char*
char* myStrCpy(char* dest,const char* src) {
    //若src的值不為空就進入迴圈
    while(*src) {
        //將src的值指派給dest，接著指標往下個位址移動
        *dest++ = *src++;
    }
    //將dest的值放上空字元
    *dest = 0;
    return dest;
}
int main() {
    char name[20];
    //清空陣列記憶體中的值
    memset(name, 0, sizeof(name));
    myStrCpy(name, "Bill");
    cout << "name = " << name << endl;
    return 0;
}
{% endhighlight %}

## memcpy

將來源字串copy到目的字串，拷貝字數由參數3決定。

```
memcpy(目的字串, 來源字串, 要拷貝字數);
```

strlen(src)回傳是不包含\'\0\'的長度，所以再+1，才會把src的結尾\'\0\'拷貝到目的字串。

{% highlight c++ linenos %}
//參數2因為是常數字串，所以型態為const char*
char* myStrCpy(char* dest,const char* src) {
    memcpy(dest, src, strlen(src) + 1);
    return dest;
}
int main() {
    char name[20];
    //清空陣列記憶體中的值
    memset(name, 0, sizeof(name));
    myStrCpy(name, "Bill");
    cout << "name = " << name << endl;
    return 0;
}
{% endhighlight %}

## 改良式strncpy

傳統的strncpy，若n的長度小於src的長度，不會在dest結尾加上\'\0\'。

所以改良式strncpy，不管n是不是小於src的長度，都會在結尾加上\'\0\'。

{% highlight c++ linenos %}
//參數2因為是常數字串，所以型態為const char*
char* myStrnCpy(char* dest,const char* src,const size_t n) {
    memcpy(dest, src, n);
    *(dest + n) = 0;//增加結尾符號
    return dest;
}
int main() {
    char name[20];
    //清空陣列記憶體中的值
    memset(name, 0, sizeof(name));
    myStrnCpy(name,"Bill",2);
    cout << "name = " << name << endl;
    return 0;
}
{% endhighlight %}