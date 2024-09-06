---
title: ASCII與中文
date: 2024-08-14
keywords: c++,ascii
---

Ascii Code 1-31為控制碼

|字元|Ascii code|
|\0|0|
|0-9|48-57|
|A-Z|65-90|
|a-z|97-122|
|空格|32|

## 印出中文Ascii code

字元char的範圍為0-255，不包含負數，只有正整數。

中文的Ascii Code 128-255，數字、大小寫英文字母、空格、標點符號、空字元(\0)介於Ascii Code 0-127。

字元char是0-255，沒有負數，在把中文字元轉換成整數之前，必須先轉成unsigned char，再轉成int，才會正確顯示中文的ascii碼。

{% highlight c++ linenos %}
(int)(unsigned char)str[i] 
{% endhighlight %}

{% highlight c++ linenos %}
int main() {
    char str[100];
    //清空字串位址的值
    memset(str, 0, sizeof(str));
    strcpy(str, "西西");
    cout << "長度:" << strlen(str) << endl;
    for(int i = 0, len = strlen(str); i < len; i++) {
        cout << "str[" << i << "] = " <<  (int)(unsigned char)str[i] << endl;
    }
    //印出字串
    cout << str << endl;
    return 0;
}
{% endhighlight %}

```
長度:6
str[0]=232
str[1]=165
str[2]=191
str[3]=232
str[4]=165
str[5]=191
西西
```

## 使用中文Ascii Code指定陣列值

{% highlight c++ linenos %}
int main() {
    char str[100];
    //清空字串位址的值
    memset(str, 0, sizeof(str));
    str[0] = 232;
    str[1] = 165;
    str[2] = 191;
    str[3] = 232;
    str[4] = 165;
    str[5] = 191;
    //印出字串
    cout << str << endl;
    return 0;
}
{% endhighlight %}

```
西西
```

## 統計中文個數


數字、大小寫英文字母、空格、標點符號、空字元(\0)介於Ascii Code 0-127。

中文的Ascii Code 128-255。


以下程式碼是錯的，仍待修正。

{% highlight c++ linenos %}
int countChar(const char* str) {
    if(str == 0) return -1;
    int count = 0;
    bool flag = false;
    while(*str) {
        //0
        if((unsigned char) *str < 128) {
            count++;
        } else {
            if(flag) {
                count++;
                flag = false;
            } else {
                flag = true;
            }
        }
        str++;
    }
    return count;
}
int main() {
    cout << countChar("西") << endl;
    return 0;
}
{% endhighlight %}