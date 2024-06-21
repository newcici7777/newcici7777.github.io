---
title: char 字串
date: 2024-06-15
keywords: c++, char array
---

## 空字元(null character)

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

以下程式碼，cstr1沒有初始化字串常數，執行程式時會從cstr1的記憶體位址開始輸出值，直到遇到記憶體位址的值為\0(空字元)才會停止輸出，不會因為超過宣告字串的長度而停止印出。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    char cstr1[21];//cstr1沒有初始化字串常數
    //遇到記憶體位址的值為\0(空字元)才會停止輸出
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


## 二維陣列字串

參考

[二維陣列]({% link _pages/c/array/array2dimen.md %})

以下的例子是建立二維的字串，總共有7個字串，每個字串最大長度為10，Wednesday是最長字串，長度為9，加上\0就等於10。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
const int DAYS = 7; //字串數，7個字串
const int MAX = 10; // 每個字串最大長度，包含\0
int main() {
    char str[DAYS][MAX] = {"Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"};
    for(int i = 0; i < DAYS; i++) {
        //印出字串
        cout << str[i] << endl;
    }
    return 0;
}
{% endhighlight %}

```
Sunday
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday

```

## 字串全設為\0

使用memset，第二個參數設為0，代表把字串的記憶體位址的值全設為\0。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    char cstr2[] = "hello";
    cout << "cstr2 長度 = " << strlen(cstr2) << ",內容 = " << cstr2 << endl;
    memset(cstr2,0,sizeof(cstr2));
    cout << "cstr2 長度 = " << strlen(cstr2) << ",內容 = " << cstr2 << endl;
    return 0;
}

{% endhighlight %}

```
執行結果
cstr2 長度 = 5,內容 = hello
cstr2 長度 = 0,內容 = 
```

## 字串拷貝 strcpy
```
char * strcpy ( char * destination, const char * source );
```
要從來源的字串，拷貝到目標的字串。

- 參數1:目標(拷貝到那裡？)
- 參數2:要拷貝的字串(src)

拷貝完成後，會自動在目標字串最後面加上\0。

{% highlight c++ linenos %}
    char c_str1[6] = {'h','e','l','\0'};
    char c_str4[20] = {'t','e'};
    strcpy(c_str4, c_str1);
    cout << "c_str4:" << c_str4 << endl;
{% endhighlight %}

```
執行結果
c_str4:hel
```

## 字元個數拷貝 strncpy
```
char *strncpy(char *string1, const char *string2, size_t count);
```
- 參數1:目標字串(拷貝到那裡？)
- 參數2:要拷貝的字串(src)
- 參數3:要拷貝多少個字元

若參數3(拷貝多少個字元)比參數2(來源字串長度)小，拷貝完成後，不會在參數1(目標字串)的結尾加上\0。

{% highlight c++ linenos %}
    char c_str4[10];
    //拷貝2個字元至c_str4
    strncpy(c_str4,"hello",2);
    cout << "c_str4[0] = " << c_str4[0] << endl;
    cout << "c_str4[1] = " << c_str4[1] << endl;
    cout << "c_str4[2] = " << c_str4[2] << endl;
    cout << "c_str4[3] = " << c_str4[3] << endl;
    cout << "c_str4[4] = " << c_str4[4] << endl;
    cout << "c_str4[5] = " << c_str4[5] << endl;
    cout << "c_str4[6] = " << c_str4[6] << endl;
    cout << "c_str4[7] = " << c_str4[7] << endl;
    cout << "c_str4[8] = " << c_str4[8] << endl;
    cout << "c_str4[9] = " << c_str4[9] << endl;
{% endhighlight %}

以下XCode可以正常執行，會在第2個字元以後補上\0(也就是ascii code = 0，也稱 null character)

```
c_str4[0] = h
c_str4[1] = e
c_str4[2] = 
c_str4[3] = 
```

VS Code執行時，不會在第2個字元以後自動補\0(也就是ascii code = 0，也稱 null character)

```
c_str4[0] = h
c_str4[1] = e
c_str4[2] = &
c_str4[3] = x00
c_str4[4] = x00
c_str4[5] = x00
c_str4[6] = x00
c_str4[7] = x00
c_str4[8] = x00
c_str4[9] = x00
```

若一開始把字串的記憶體位址的值全設為ascii code 0，就不會出現上述問題。
{% highlight c++ linenos %}
    char c_str4[10] = {0};
    strncpy(c_str4,"hello",2);
{% endhighlight %}


## 字串長度 strlen

strlen()是計算字串中有幾個字元，不包含字串結尾\0。

sizeof()是計算字串變數全部記憶體大小。

{% highlight c++ linenos %}
int main() {
    char c_str4[10] = "hello!";
    cout << "c_str4 size:" << sizeof(c_str4) << endl;
    cout << "c_str4長度:" << strlen(c_str4) << endl;
    return 0;
}
{% endhighlight %}

```
c_str4 size:10
c_str4長度:6
```

## 字串連接 strcat

{% highlight c++ linenos %}
    char c_str1[6] = {'h','e','l','\0'};
    //[]可為空
    char c_str2[] = {'h','e','l','l','o','\0'};
    char c_str3[10];
    char c_str4[20] = {'t','e'};
    strcpy(c_str3, c_str1);
    cout << "c_str3 + c_str4 = " << strcat(c_str3,c_str4) << endl;
{% endhighlight %}

```
執行結果
c_str3 + c_str4 = helte
```
