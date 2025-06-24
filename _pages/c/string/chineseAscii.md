---
title: ASCII與中文
date: 2024-08-14
keywords: c++, ascii, utf-8, unicode
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
寫一個程式，計算中文個數，一個中文字算1個，一個英文字算1個。

數字、大小寫英文字母、空格、標點符號、空字元(\0)介於Ascii Code 0-127。

中文的Ascii Code 128-255。

不同的編碼方式中文個數不同。

- utf-8 中文3個byte，英文1個byte。
- unicode 中文2個byte，英文2個byte。

從之前的程式碼範例，「西西」二個中文字，但卻產生6個char，C++ 1個char是1byte，「西西」二個字占6byte，一個中文字占3byte，代表目前我的編譯器是使用utf-8。

以下程式碼max_chinese_cnt = 3，代表一個中文字使用3個byte。

chinese_cnt計算byte數量，chinese_cnt = 1，由1開始，加到第3個byte代表一個中文字讀取完畢，可以count++。

{% highlight c++ linenos %}
int countChar(const char* str) {
  if(str == 0) return -1;
  int count = 0;  // 統計字數，一個中文字算1個，一個英文字算1個
  int chinese_cnt = 1;  // 由1開始
  int max_chinese_cnt = 3;  // 一個中文字使用3個byte
  for(int i = 0, len = strlen(str); i < len; i++) {
    // cout << "str[" << i << "] = " <<  (int)(unsigned char)str[i] << endl;
    unsigned int ch = (int)(unsigned char)str[i];
    if(ch < 128) {  // 英文字母走這邊
      count++;
    } else {  // 中文走這邊
      // 加到第3個byte代表一個中文字讀取完畢，可以count++
      if (chinese_cnt == max_chinese_cnt) {
        count++;
        chinese_cnt = 1;
      } else {
        // 常未讀取完一個中文字
        chinese_cnt++;
      }
    }
  }
  return count;
}
int main() {
  cout << countChar("西西abc") << endl;
  return 0;
}  
{% endhighlight %}
```
5
```

以下是另外一種寫法。
{% highlight c++ linenos %}
int countChar(const char* str) {
  if(str == 0) return -1;
  int count = 0;
  int chinese_cnt = 1;
  int max_chinese_cnt = 3;
  while(*str) {
    //0
    if((unsigned char) *str < 128) {
      count++;
    } else {
      if (chinese_cnt == max_chinese_cnt) {
        count++;
        chinese_cnt = 1;
      } else {
        chinese_cnt++;
      }
    }
    str++;
  }
  return count;
}
int main() {
  cout << countChar("西西abc") << endl;
  return 0;
}
{% endhighlight %}
```
5
```