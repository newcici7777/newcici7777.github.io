---
title: 字元搜尋
date: 2024-08-02
keywords: c++, strchr 
---

## strchr
{% highlight c++ linenos %}
char* mystrchr(const char* s,int c) {
    //將s開始位址給p指標
    //const char*轉成char*要強制轉型
    char* p = (char *)s;
    //若p指標的值不為\0就進入迴圈
    while(*p) {
        //若為指標的值與字元是相同，代表找到
        //傳回p的位址
        if(*p == c) return p;
        p++;//位址往右移一位
    }
    return 0;//找不到就回傳null的位址
}
int main() {
    char str[20];
    //清空陣列記憶體中的值
    memset(str, 0, sizeof(str));
    strcpy(str, "Hello World!");
    char* p1 = mystrchr(str,'W');
    if(p1 != 0) {
        cout << "p1 = " << p1 << endl;
    }
    return 0;
}
{% endhighlight %}

```
p1 = World!
```

## strrchr

{% highlight c++ linenos %}
char* mystrrchr(const char* s,int c) {
    //將s開始位址給p指標
    //const char*轉成char*要強制轉型
    char* p = (char *)s;
    char* p1 = 0;
    //若p指標的值不為\0就進入迴圈
    while(*p) {
        //若為指標的值與字元是相同，代表找到
        //p的位址放入p1中
        if(*p == c) p1 = p;
        p++;//位址往右移一位
    }
    return p1;//回傳p1最後的位址
}
int main() {
    char str[20];
    //清空陣列記憶體中的值
    memset(str, 0, sizeof(str));
    strcpy(str, "Hello World!");
    char* p1 = mystrrchr(str,'o');
    if(p1 != 0) {
        cout << "p1 = " << p1 << endl;
    }
    return 0;
}
{% endhighlight %}

```
p1 = orld!
```