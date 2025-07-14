---
title: 刪除字元
date: 2024-08-08
keywords: c++, deleteRchr ,deleteLchr 
---

Prerequisites:

- [記憶體間隔計算][1]

## 刪除右邊多的字元

刪除前字串:Hellloyyyoooyyy

刪除右邊有y的字母

刪除後字串:Hellloyyyooo

### 思路

添加flag，判斷找到右邊的字元，flag變成1，並記錄位置，但如果右邊字元後面仍有其它字母就把flag變成0。

第一次找到y，flag變1，並記錄位址。

|H|e|l|l|l|o|y|y|y|o|o|o|y|y|y|
| | | | | | |1| | | | | | | | |
| | | | | | |p| | | | | | | | |

發現後面有不是y的字母，flag變0。

|H|e|l|l|l|o|y|y|y|o|o|o|y|y|y|
| | | | | | | | | |0| | | | | |
| | | | | | |p| | | | | | | | |

第二次找到y，flag變1，並記錄位址。

|H|e|l|l|l|o|y|y|y|o|o|o|y|y|y|
| | | | | | | | | | | | |1| | |
| | | | | | | | | | | | |p| | |

將記錄的位址變成\'\0\'結尾空字元，也就是0。

|H|e|l|l|l|o|y|y|y|o|o|o|0|y|y|
| | | | | | | | | | | | |1| | |
| | | | | | | | | | | | |p| | |

當印出字串時看到有結尾空字元0，就會停止輸出。

印出結果

Hellloyyyooo

{% highlight c++ linenos %}
void deleterchr(char* str, const int c) {
  //判斷位址為null就返回
  if(str == 0) return;
  //記錄位址的指標
  char *p = 0;
  //false為0，true為1，預設為0
  bool flag = false;
  //若字元不為空字元就進入迴圈
  while(*str) {
    //如果字元的值等於要刪的字母
    if(*str == c) {
      //如果flag為0，才能進來
      if(!flag) {
        //記錄要刪字母的位置
        p = str;
        //把flag變成1
        flag = true;
      }
    } else {
      //如果要刪的字母後面仍有其它不能刪的字母
      //直接把flag變成0
      flag = false;
    }
    //指標往下個位址移動
    str++;
  }
  //若有找到刪除的字元
  if(flag) {
    //把之前記錄的位址的值變成0
    *p = 0;
  }
}
int main() {
  char str[100];
  //清空字串位址的值
  memset(str, 0, sizeof(str));
  strcpy(str, "Hellloyyyoooyyy");
  //刪除右邊為y的字母
  deleterchr(str, 'y');
  //印出刪除後的字串
  cout << str << endl;
  return 0;
}
{% endhighlight %}
```
Hellloyyyooo
```

## 刪除左邊的字元

刪除前字串:yyyyooooyy

刪除左邊有y的字母

刪除後字串:ooooyy

### 思路

#### 找出第一個不是刪除字元的記憶體位址。

count變數，記錄有幾個字元要刪除。

p指標，主要記錄第一個不是要刪的字母。

p指標，初始化為字串陣列索引0的記憶體位址。

訪問字串陣列的每一個字元，若等於要刪除的字元，就進入迴圈，然後p指標往下一個索引移動，若下一個字元等於刪除的字元，然後就進入迴圈，直到下一個字元不等於刪除的字元就離開迴圈，而p指標所指向的就是第一個不等於y的字元，同時count變數也會記錄要刪除的個數，每進入一次迴圈就代表要刪掉一個字元。

|字串|y|y|y|y|o|o|o|o|y|y|
|p指標| | | | |p| | | | | |
|count|0|1|2|3|4| | | | | |

#### 將後面的字串往前移動至最前面

|字串   |y|y|y|y|o|o|o|o|y|y|
|覆蓋的字串|o|o|o|o|y|y| | | | |

因為有覆蓋到原本的字串，所以要使用memmove

```
memmove(目的位址, 來源位址, 拷貝個數);
```

#### 程式碼1

{% highlight c++ linenos %}
void deletelchr(char* str, const int c) {
  if(str == 0) return;
  //p指標初始化str字串首位址
  //p指標，記錄第一個不是要刪的字母。
  char *p = str;
  //記錄有幾個字元要刪除
  int count = 0;
  //若當前字元為要刪除的c則進入迴圈
  while(*p == c) {
    //指標移到下一個字元
    p++;
    count++;
    //離開迴圈的條件是字元不等於c
    //p會指向非刪除的字元
  }
  //若p的位址，不等於str的首位址
  //代表有進入上面while的迴圈
  if(p != str) {
    //把來源字串移到目的字串
//    memmove(str, p, strlen(str) - (p - str) + 1);
     //+1是把結尾空字元符號\0也拷貝過來
     memmove(str, p, strlen(str) - count + 1);
  }
}
int main() {
  char str[100];
  //清空字串位址的值
  memset(str, 0, sizeof(str));
  strcpy(str, "yyyyooooyy");
  
  deletelchr(str, 'y');
  //印出刪除後的字串
  cout << str << endl;
  return 0;
}
{% endhighlight %}

```
ooooyy
```

#### 程式碼2


|字串|y|y|y|y|o|o|o|o|y|y|
|str指標|str | | | | | | | | | |
|p指標| | | | |p| | | | | |


```
memmove(目的位址, 來源位址, 拷貝個數);
```

```
memmove(str, p, strlen(str) - (p - str) + 1);
```

假設str的位址是0x00000004

假設p的位址是0x00000008

p - str = 4

strlen("yyyy0000yy") = 10

```
memmove(str, p, 10 - 4 + 1);
```

最後的+1，是把來源字串的\'\0\'也覆蓋目的字串。

也就是把\'ooooyy\0\'共7個覆蓋到目的位址。

{% highlight c++ linenos %}
void deletelchr(char* str, const int c) {
  if(str == 0) return;
  //p指標存放str字串首位址
  char *p = str;
  //若當前字元為要刪除的c則進入迴圈
  while(*p == c) {
    //指標移到下一個字元
    p++;
    //離開迴圈的條件是字元不等於c
    //p會指向非刪除的字元
  }
  //若p的位址，不等於str的首位址
  //代表有進入上面while的迴圈
  if(p != str) {
    //把來源字串移到目的字串
    //+1是把結尾空字元符號\0也拷貝過來
    memmove(str, p, strlen(str) - (p - str) + 1);
  }
}
int main() {
  char str[100];
  //清空字串位址的值
  memset(str, 0, sizeof(str));
  strcpy(str, "yyyyooooyy");
  
  deletelchr(str, 'y');
  //印出刪除後的字串
  cout << str << endl;
  return 0;
}
{% endhighlight %}

## 刪除中間字串

刪除前字串 = zzz123zzz

要刪除123

刪除後字串 = zzzzzz

### 指標解釋

str字串陣列第0個記憶體位址

find要刪除字串的第一個記憶體位址

slen為要刪除字串的長度，要刪除`123`，刪除字串大小為3

find+slen排除掉刪除字串的第一個記憶體位址。

|z|z|z|1|2|3|z|z|z|0|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|str| | |find| | |find+slen| | | |

### 思路

聚焦在find要刪除字串。

### 把find字串覆蓋前後表格

|覆蓋前|1|2|3|z|z|z|0|
|第一個位址|find| | | | | | |
|覆蓋後|z|z|z|0| | | |

### find與find+slen指標位址

|1|2|3|z|z|z|0|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|find| | |find+slen| | | |

目的字串位址是find = 123zzz0

來源字串位址是find + slen(3) = zzz0

要覆蓋幾個字元？find字串長度(6) - 要刪字串的長度(3) + \'\0\' 空字元(1) = 4個字元

```
memmove(目的位址, 來源位址, 拷貝個數);
```

```
memmove(find, find + slen, strlen(find) - slen + 1);
```

### 程式碼(while)

{% highlight c++ linenos %}
void delMiddle(char* str, const char* substr) {
  if(str == 0 || substr == 0) return;
  if(strlen(str) == 0) return;
  int slen = strlen(substr);
  if(slen == 0) return;
  while(true) {
    char* find = strstr(str, substr);
    if(find == 0) return;
    memmove(find, find + slen, strlen(find) - slen + 1);
  }
  //遞迴作法
  //delMiddle(find, substr);
}
int main() {
  char str[100];
  //清空字串位址的值
  memset(str, 0, sizeof(str));
  strcpy(str, "zzz123123zzz");
  delMiddle(str, "123");
  //印出刪除後的字串
  cout << str << endl;
  return 0;
}
{% endhighlight %}

```
zzzzzz
```

### 程式碼(遞迴)

{% highlight c++ linenos %}
void delMiddle(char* str, const char* substr) {
  if(str == 0 || substr == 0) return;
  if(strlen(str) == 0) return;
  int slen = strlen(substr);
  if(slen == 0) return;
  char* find = strstr(str, substr);
  if(find == 0) return;
  memmove(find, find + slen, strlen(find) - slen + 1);
  //遞迴作法
  delMiddle(find, substr);
}
int main() {
  char str[100];
  //清空字串位址的值
  memset(str, 0, sizeof(str));
  strcpy(str, "zzz123123zzz");
  delMiddle(str, "123");
  //印出刪除後的字串
  cout << str << endl;
  return 0;
}
{% endhighlight %}

```
zzzzzz
```

[1]: {% link _pages/c/dynamicMemory/memory_interval.md %}