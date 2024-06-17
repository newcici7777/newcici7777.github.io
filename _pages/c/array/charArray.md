---
title: char 字串
date: 2024-06-15
keywords: c++, char array
---

## 空字元(nullcharacter)

char字串必須以'\0'做結尾，若不是'\0'做結尾則稱為char陣列。

宣告字串，必須留1個字元，放結尾\0，以下宣告最多可以放5個字元，而第6個字元則是放\0。
```
char str[6];
```

以下二種宣告是完全不同，一個是字串，一個是陣列。
{% highlight c++ linenos %}
    //char字串
    // \0 代表結尾
    char str1[6] = {'h','e','l','l','o','\0'};
    cout << "str1 長度 = " << strlen(str1) << ",內容 = " << str1 << endl;
    //char 陣列
    char arr[6] = {'h','e','l','l','o','o'};
    cout << "arr 長度 = " << strlen(arr) << ",內容 = " << arr << endl;
    cout << "arr sizeof = " << sizeof(arr) << endl;
{% endhighlight %}

```
執行結果
str1 長度 = 5,內容 = hello
arr 長度 = 11,內容 = helloohello
arr sizeof = 6
```
## 字串常數

注意!以下""雙引號包住的字串是常數，常數代表不可以修改，編譯器會自動加上'\0'作結尾，不用手動加'\0'。

{% highlight c++ linenos %}
	//字串常數
    char str1[] = "hello"
{% endhighlight %}

## 字串常數宣告

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    char cstr1[21];
    cout << "cstr1 長度 = " << strlen(cstr1) << ",內容 = " << cstr1 << endl;
    //以下結尾編譯器會自動加上\0
    char cstr2[] = "hello";
    cout << "cstr2 長度 = " << strlen(cstr2) << ",內容 = " << cstr2 << endl;
    char cstr3[6] = "hello";
    cout << "cstr3 長度 = " << strlen(cstr3) << ",內容 = " << cstr3 << endl;
    char cstr4[] = {"hello"};
    cout << "cstr4 長度 = " << strlen(cstr4) << ",內容 = " << cstr4 << endl;
    char cstr5[6] = {"hello"};
    cout << "cstr5 長度 = " << strlen(cstr5) << ",內容 = " << cstr5 << endl;
    char cstr6[6] {"hello"};
    cout << "cstr6 長度 = " << strlen(cstr6) << ",內容 = " << cstr6 << endl;
    //設為nullptr
    char cstr7[6] = {0};
    cout << "cstr7 長度 = " << strlen(cstr7) << ",內容 = " << cstr7 << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
cstr1 長度 = 6,內容 = p\364\357\277\367
cstr2 長度 = 5,內容 = hello
cstr3 長度 = 5,內容 = hello
cstr4 長度 = 5,內容 = hello
cstr5 長度 = 5,內容 = hello
cstr6 長度 = 5,內容 = hello
cstr7 長度 = 0,內容 = 
```
cstr1沒有初始化字串，執行程式時會從cstr1的記憶體位址開始印值，直到遇到記憶體位址的值為\0(空字元)才會停止印出，不會因為超過宣告字串的長度而停止印出。

## 字串設為nullptr

#include <iostream>
using namespace std;
int main() {
    char cstr2[] = "hello";
    cout << "cstr2 長度 = " << strlen(cstr2) << ",內容 = " << cstr2 << endl;
    memset(cstr2,0,sizeof(cstr2));
    cout << "cstr2 長度 = " << strlen(cstr2) << ",內容 = " << cstr2 << endl;
    return 0;
}

```
執行結果
cstr2 長度 = 5,內容 = hello
cstr2 長度 = 0,內容 = 
```
