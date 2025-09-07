---
title: char字串指標
date: 2024-06-19
keywords: c++, char array
---
## 字串常數
### 字串常數是包在二個雙引號之間""
```
"http://www.google.com"
```

以下皆為字串常數

- 例1

	s指標指向字串常數，常數占在記憶體唯讀空間，常數是放在RODATA記憶體區塊，只能讀取，無法修改。

{% highlight c++ linenos %}
char * s = "http://www.google.com";
{% endhighlight %}

- 例2

	程式碼中的"abcdefg"是字串常數。

{% highlight c++ linenos %}
//使用new動態配置記憶體空間，new會傳回char陣列記憶體區塊的開始記憶體位址。
char* message = new char[100];
//把字串常數abcdefg拷貝至char陣列
strcpy(message, "abcdefg");
//記憶體回收
delete[] message;
//將指標指向null，代表不指向任何記憶體位址
message = nullptr;
{% endhighlight %}

### 無法透過\*取值運算子，把字串常數指派給指標
無法透過\*取值運算子，把字串常數指派給指標，以下會編譯錯誤，只能使用strcpy複製字串常數。

{% highlight c++ linenos %}
char* message = new char[100];
* message = "abcdefg";//編輯錯誤
{% endhighlight %}

### 字串常數記憶體大小

因為常數非動態記憶空間，所以要透過strlen來計算記憶體空間大小，注意！strlen不包含結尾\0的大小，所以計算記憶體空間要再+1，加上結尾\0的大小
{% highlight c++ linenos %}
  char* s = "http://www.google.com";//21個字元(不含結尾\0)
  //因為常數非動態記憶空間，所以要透過strlen來計算記憶體空間大小
  size_t s_size = strlen(s) + 1;//strlen不包含結尾\0的大小
{% endhighlight %}

### 字元陣列不是常數

字元陣列不是常數，以下寫法不是常數，是有占stack堆疊的記憶體空間
{% highlight c++ linenos %}
  char s1[] = "http://www.google.com";
  cout << "s1堆疊記憶體大小=" << sizeof(s1) << endl;
  cout << "s1長度=" << strlen(s1) << endl;
{% endhighlight %}

```
s1記憶體大小=22
s1長度=21
```

## 指向字串常數的指標
以下程式分別為字串陣列與指標初始化字串常數，不可以使用`字串陣列名++`，因為陣列名是指向陣列記憶體的開始位址，到程式結束前都不會改變，陣列名是常數，就像我們不可能寫成`7++`的意思是一樣的，而指標是變數，變數就是可以改變值(在這裡是改變存放的記憶體位址)。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  //cstr1陣列名是記憶體起始位址
  char cstr1[] = "Hello World!";
  //cstr2是指標變數，指向字串常數"Hello World!"
  //常數是放在code segment 記憶體區塊，不可以使用指標指向常數記憶體位址
  char* cstr2 = "Hello World!";
  
  cout << "cstr1 = " << cstr1 << endl;
  cout << "cstr2 = " << cstr2 << endl;
  
  //cstr1陣列名是記憶體起始位址，是常數，不可以++
  //cstr1++;
  
  //指標是變數，可以修改
  //指標原本指向起始位址"H"，++移動1byte，指標指向"e"的記憶體位址
  cstr2++;
  
  //印出ello World!，直到遇到\0結尾字元，就停止輸出
  cout << "cstr2 = " << cstr2 << endl;
  return 0;
}
{% endhighlight %}

```
cstr1 = Hello World!
cstr2 = Hello World!
cstr2 = ello World!
```

## 字串指標與函式
寫一個傳進指標的函式，函式是把來源字串(src)拷貝到目標字串(desc)。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void copystr(char* desc, char* src) {
  //若指標指向的記憶體位址的值是\0就離開while
  //若指標指向的記憶體位址的值不是\0就進入while
  while(*src) {
    //取出src指標指向的記憶體位址的內容
    //使用*取值運算子修改desc指向記憶體位址的*
    *desc = *src;
    desc++;//指標往下一個位址移動，移動1byte
    src++;//指標往下一個位址移動，移動1byte
  }
  //目標字串放上結尾字元，代表字串結束
  *desc = '\0';
}
int main() {
  //拷貝的來源字串
  char* src = "Hello World!";
  //+1是為了加上結尾空字元\0，strlen預設是不含\0
  size_t len = strlen(src) + 1;
  //拷貝的目標字串
  char* desc = new char[len];
  
  copystr(desc, src);
  cout << "desc = " << desc << endl;
  
  //記憶體釋放陣列
  delete[] desc;
  //desc指標不指向任何記憶體位址
  desc = nullptr;
  return 0;
}
{% endhighlight %}

```
desc = Hello World!
```

另一種寫法
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void copystr(char* desc, char* src) {
  //若指標指向的記憶體位址的值是\0就離開while
  //若指標指向的記憶體位址的值不是\0就進入while
  while(*src) {
    //取出src指標指向的記憶體位址的內容
    //使用*取值運算子修改desc指向記憶體位址的*
    //desc與src指標往下一個位址移動，移動1byte
    *desc++ = *src++;
  }
  //目標字串放上結尾字元，代表字串結束
  *desc = '\0';
}
int main() {
  //拷貝的目標字串
  char desc[100];
  //拷貝的來源字串
  char* src = "Hello World!";
  copystr(desc, src);
  cout << "desc = " << desc << endl;
  return 0;
}
{% endhighlight %}

## 指標字串拷貝
```
char * strcpy ( char * destination, const char * source );
```

使用函式庫strcpy來進行字串拷貝，參數2指標前面有const，代表strcpy()函式不能修改source指標所指向的字元，因為指向的字元是常數，但可以改變指標src指向的位址。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  //拷貝的目標字串
  char desc[100];
  //拷貝的來源字串
  char* src = "Hello World!";
  strcpy(desc, src);
  cout << "desc = " << desc << endl;
  return 0;
}
{% endhighlight %}

```
desc = Hello World!
```



