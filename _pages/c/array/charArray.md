---
title: c字串
date: 2024-06-15
keywords: c++, char array
---
## 字串陣列
語法
```
char 陣列名[大小];
```

### 陣列名
陣列名c，預設是索引[0]的記憶體位址。<br>
所以印出c與`&c[0]`，結果都是相同。<br>
`&c`代表整個陣列，這個只有在指標移動的時候才會看出差異。<br>
`&c + 1`是移動整個陣列，`c + 1`從索引[0]移動到索引[1]。<br>
{% highlight c++ linenos %}
int main() {
  char c[7] = {'a', 'b', 'c'};
  printf("c address = %p \n" ,c);
  printf("c[0] address = %p \n", &c[0]);
  printf("&c address = %p \n", &c);
  return 0;
}
{% endhighlight %}
```
c address = 0x7ff7bfeff2e5 
c[0] address = 0x7ff7bfeff2e5 
&c address = 0x7ff7bfeff2e5 
```

### 陣列初始值
C語言支持只給部分陣列元素設初始值。<br>
以下只有abc有值，其它都為0，若初始化的值小於陣列大小，其它值都會設為0。<br>
{% highlight c++ linenos %}
int main() {
  char c[7] = {'a', 'b', 'c'};
  int len = sizeof(c) / sizeof(char);
  for(int i = 0; i < len; i++) {
    printf("i = %d, value = %d \n", i, c[i]);
  }
  return 0;
}
{% endhighlight %}
```
i = 0, value = 97 
i = 1, value = 98 
i = 2, value = 99 
i = 3, value = 0 
i = 4, value = 0 
i = 5, value = 0 
i = 6, value = 0 
```

### 印出字串陣列
以下陣列大小7，但有值的元素只有abc，C語言印出字串陣列，直接給陣列名，陣列名也就是索引[0]的記憶體位址。C語言會先印出記憶體位址中的字元，接著指標往下一個索引移動，直到遇到0，就會停止印出。<br>

C語言印出字串是使用%s格式化字元。<br>
C++是使用cout。<br>
重點是，以下程式碼只使用陣列名c，而不是&c。
{% highlight c++ linenos %}
int main() {
  char c[7] = {'a', 'b', 'c'};
  cout << c << endl;
  printf("%s \n", c);
  return 0;
}
{% endhighlight %}
```
abc
abc 
```

#### `\0`
整數0在char代表`\0`，代表結束讀取。

|二進制|十進制|跳脫字元|表示|
|0000 0000|0|`\0`|空字元（Null)|

![img]({{site.imgurl}}/c++/arr/char_arr1.png)<br>

{% highlight c++ linenos %}
int main() {
  char c[7] = {'a', 'b', 'c'};
  printf("c = %s \n",c);
  return 0;
}
{% endhighlight %}
```
c = abc
```
#### `\0`無法印出
使用%c，無法印出`\0`，也就是整數是0，字串的結尾。<br>
上個程式範例，無法印出`\0`。<br>
但如果是轉成%d，就可以印出整數。<br>
C++中，char是整數，可以用%d或%c印出，%d是印出整數，%c是印出整數對映的ASCII Code。<br>

#### 初始化的字元數量與陣列大小一樣
若是初始化的字元數量與陣列大小一樣，不會自動在後面補0，其它編譯器會印出亂碼，因為印出直到「遇到0」為止，那沒有遇到0之前，就一直印。<br>

![img]({{site.imgurl}}/c++/arr/char_arr2.png)<br>

{% highlight c++ linenos %}
int main() {
  char c[3] = {'a', 'b', 'c'};
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}

需手動加上`\0`，代表遇到0就停止不要印出。
{% highlight c++ linenos %}
int main() {
  char c[4] = {'a', 'b', 'c', '\0'};
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}
```
abc
```
#### 沒設定陣列大小，後面不會補0
以下沒設定陣列大小，也不會自動後面補\0。<br>
![img]({{site.imgurl}}/c++/arr/char_arr2.png)<br>
{% highlight c++ linenos %}
int main() {
  char c[] = {'a', 'b', 'c'};
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}

### 字串陣列可修改內容
字串陣列可修改內容。<br>
{% highlight c++ linenos %}
int main() {
  char c[7] = {'a', 'b', 'c'};
  c[0] = 'g';
  c[1] = 'g';
  int len = sizeof(c) / sizeof(char);
  for(int i = 0; i < len; i++) {
    printf("i = %d, value = %c \n", i, c[i]);
  }
  return 0;
}
{% endhighlight %}
```
i = 0, value = g 
i = 1, value = g 
i = 2, value = c 
i = 3, value = 
i = 4, value = 
i = 5, value = 
i = 6, value = 
```

### 字元是整數
字元本身是整數，可以運算，將陣列大小26，依序存入A \~ Z，26個英文大寫字母。<br>
ascii code是把整數轉成字元，輸出在螢幕上，字元的存在記憶體中是整數，不是英文字母。<br>
{% highlight c++ linenos %}
int main() {
  char arr[26];
  int i;
  for (int i = 0; i < 26; i++) {
    arr[i] = 'A' + i;
    cout << arr[i] << ", ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}

### 字串常數
```
char c[] = "abc";
```
等號後面是「常數」，常數會在最後自動補上`\0`，常數存在memory layout中的RODATA區塊中。<br>
以下程式碼，會在stack建立陣列大小為4(包含`\0`)的位置，把RODATA中的常數，「複製」到c`[]`陣列中，所以可以修改`c[]`陣列中的值。
![img]({{site.imgurl}}/c++/arr/char_arr3.png)<br>

以下二種方式都可以，可以設定大小或不設定大小。
```
char c[] = "字串內容"
char c[大小] = "字串內容"
```
讀取<br>
{% highlight c++ linenos %}
int main() {
  char c[] = "abc";
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}
```
abc
```

雖然c陣列指向的是字串常數，但實際上是把RODATA的字串常數，逐一複製到Stack中的char字串陣列中的位址。<br>
所以可以修改字串陣列中的內容。<br>
{% highlight c++ linenos %}
int main() {
  char c[] = "abc";
  printf("%s \n",c);
  c[1] = 'g';
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}
```
abc 
agc 
```

#### 字串陣列無法重新設成新的字串常數
所謂陣列，是「固定大小」，無法再改變大小。<br>
以下宣告大小為7的陣列，已經把RODATA的常數ABC複製到Stack中的arr陣列中。<br>
無法再複製RODATA的常數DEFG，重新複製到Stack中的arr陣列中。<br>
{% highlight c++ linenos %}
int main() {
  char arr[7] = "ABC";
  arr = "DEFG";
  return 0;
}
{% endhighlight %}

就算abc的字串與ggg的字串長度一樣，也無法設成新的字串常數。<br>
{% highlight c++ linenos %}
int main() {
  char c[] = "abc";
  printf("%s \n",c);
  c = "ggg"; 
  return 0;
}
{% endhighlight %}

#### 大括號中的常數
`{}`大括號裡面是常數。<br>
{% highlight c++ linenos %}
char cstr4[] = {"hello"};
{% endhighlight %}

#### 字串陣列的常數
注意!以下""雙引號包住的字串是常數，編譯器會自動加上'\0'作結尾，不用手動加'\0'。
{% highlight c++ linenos %}
//字串常數
char str1[] = "hello"
{% endhighlight %}

## 字串指標
以下是指標的方式建立字串。
```
char *c = "字串內容"
```
「c指標」指向RODATA區塊的「字串常數」，RODATA是唯讀區塊，無法修改字串常數的內容，但可以修改指標指向到新的字串常數。。<br>

![img]({{site.imgurl}}/c++/arr/char_arr4.png)<br>

### 字串指標加上const
因為指向的RODATA已經是字串常數，建議在char* 前加上const常數宣告，編譯器才不會出現警告，字串指標加上const代表無法修改RODATA的字元，但可以指向其它常數字串。<br>
下面程式碼，加上const無法修改\"abc\"常數字串，指標名c，可以指向其它新的常數字串。<br>

{% highlight c++ linenos %}
int main() {
  const char* c = "abc";
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}

RODATA區塊的字串常數無法修改。<br>
以下會編譯錯誤。<br>

{% highlight c++ linenos %}
int main() {
  constant char* c = "abc";
  c[1] = g;
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}

指標可以指向其它字串。<br>
{% highlight c++ linenos %}
int main() {
  constant char* c = "abc";
  c = "zzzz";
  printf("%s \n",c);
  return 0;
}
{% endhighlight %}
```
zzz
```

以下""雙引號包住的字串是常數，編譯器會自動加上'\0'作結尾，不用手動加'\0'。
{% highlight c++ linenos %}
//指標指向字串常數
char* str1 = "hello"
{% endhighlight %}

指標有自己的記憶體位址，存的是RODATA的字串常數索引[0]記憶體位址。<br>
{% highlight c++ linenos %}
int main() {
  char* p = "Hello";
  printf("p的address = %p 儲存的address = %p \n",&p , p);
  return 0;
}
{% endhighlight %}
```
p的address = 0x7ff7bfeff2e0 儲存的address = 0x100003f7e 
```

以下為舊的文章內容。
----------------

## 空字元(null character)
char字串必須以'\0'做結尾，若不是'\0'做結尾則稱為char陣列。<br>
宣告字串，必須留1個字元，放結尾\0，以下宣告最多可以放5個字元，而第6個字元則是放\0。<br>
```
char str[6];
```

以下二種宣告是完全不同，一個是字串，一個是陣列。<br>
{% highlight c++ linenos %}
  //char字串
  // \0 代表結尾
  char str1[6] = {'h','e','l','l','o','\0'};
  cout << "str1 長度 = " << strlen(str1) << ",內容 = " << str1 << endl;
{% endhighlight %}
```
執行結果
str1 長度 = 5,內容 = hello
```
## sizeof與strlen
sizeof(arr)是計算陣列的大小。<br>
strlen(arr)是計算有多少字元。<br>
{% highlight c++ linenos %}
  //char 陣列
  char arr[6] = {'h','e','l'};
  cout << "arr 長度 = " << strlen(arr) << ",內容 = " << arr << endl;
  cout << "arr sizeof = " << sizeof(arr) << endl;
{% endhighlight %}
```
執行結果
arr 長度 = 3,內容 = hel
arr sizeof = 6
```

### 字串長度 strlen
strlen()是計算字串中有幾個字元，不包含字串結尾\0。<br>
sizeof()是計算字串變數全部記憶體大小。<br>
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

### 字串指標不支持sizeof()
```
char* 字串指標 = "常數";
char* str = "Hello";
```
以上是宣告字串指標。<br>
若想得到常數的記憶體位址或記憶體大小，sizeof(str)，得到是8byte，因為字串指標大小是8byte。<br>

#### strlen()+1
由於sizeof(字串指標)，只會得到8byte<br>
改用strlen(字串指標) + 1，1是包含`\0`，strlen()函式預設不包含`\0`，需手動自己加上。
{% highlight c++ linenos %}
int main() {
  char* cstr = "Hello";
  cout << sizeof(cstr) << endl;
  cout << strlen(cstr) + 1 << endl;
}
{% endhighlight %}
```
8
6
```
以上結果得知，"Hello"的大小，若包含結尾`\0`，大小則為6。

#### strlen(字串常數)+1
也可以自己進行檢查。<br>
以下程式碼，不包含字串結束`\0`
{% highlight c++ linenos %}
  cout << strlen("Hello") << endl;
{% endhighlight %}
```
5
```

### 字串常數宣告
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

## 字串清空
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
要從來源的字串，拷貝到目的字串。

- 參數1:目的字串記憶體位址(拷貝到那裡？)
- 參數2:來源字串記憶體位址(要拷貝的字串)

拷貝完成後，會自動在目的字串最後面加上\0。

陣列本身就是記憶體位址。
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

## 字串指標拷貝 strcpy
{% highlight c++ linenos %}
  char* c_str1 = new char[100];
  strcpy(c_str1, "abcdefg");
  char* c_str2 = new char[100];
  strcpy(c_str2, c_str1);
  cout << "c_str1 = " << c_str1 << endl;
  cout << "c_str2 = " << c_str2 << endl;
{% endhighlight %}

```
c_str1 = abcdefg
c_str2 = abcdefg
```

## 字串修改
陣列名，雖然存放索引[0]的記憶體位址，但陣列名不是指標，無法重新指派新的字串常數記憶體位址。
{% highlight c++ linenos %}
  char c_str1[6] = "Hello";
  c_str1 = "abc";
{% endhighlight %}

若是char指標，就可以重新指派新的字串常數記憶體位址。
{% highlight c++ linenos %}
int main() {
  char* cstr = "Hello";
  cout << cstr << endl;
  cstr = "ABCDEF";
  cout << cstr << endl;
}
{% endhighlight %}
```
Hello
ABCDEF
```

使用strcpy修改字串陣列。
{% highlight c++ linenos %}
  char c_str1[6] = "Hello";
  cout << "Before = " << c_str1 << endl;
  strcpy(c_str1,"Dog");
  cout << "After = " << c_str1 << endl;
{% endhighlight %}
```
Before = Hello
After = Dog
```

## 字元個數拷貝 strncpy
```
char *strncpy(char *string1, const char *string2, size_t count);
```
- 參數1:目的字串(拷貝到那裡？)
- 參數2:來源字串(要拷貝的字串)
- 參數3:要拷貝多少個字元

若參數3(拷貝多少個字元)比參數2(來源字串長度)小，拷貝完成後，不會在參數1(目的字串)的結尾加上\0。

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

## 字串連接 strcat
```
char * strcpy ( char * destination, const char * source );
```
目的字串「連接」來源字串。

- 參數1:目的字串記憶體位址
- 參數2:來源字串記憶體位址

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

## 字串比較 strcmp
二個字串，字元逐字元比較ascii code，直到比完或分出大小為止。
```
strcmp(s1,s2)
```

### s1==s2傳回0
{% highlight c++ linenos %}
int main() {
  char* s1 = "abc";
  char* s2 = "abc";
  cout << strcmp(s1,s2) << endl;
  return 0;
}
{% endhighlight %}
```
0
```

### s1>s2傳回正數ascii code
{% highlight c++ linenos %}
int main() {
  char* s1 = "abc";
  char* s2 = "ab";
  cout << strcmp(s1,s2) << endl;
  return 0;
}
{% endhighlight %}
```
99
```

### s1<s2傳回負數ascii code
{% highlight c++ linenos %}
int main() {
  char* s1 = "ab";
  char* s2 = "abc";
  cout << strcmp(s1,s2) << endl;
  return 0;
}
{% endhighlight %}
```
-99
```

### 印出禮拜一至禮拜天的英文字母
以下的例子是建立二維的字串，總共有7個字串，每個字串最大長度為10，Wednesday是最長字串，長度為9，加上\0就等於10。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
const int DAYS = 7; //字串數，7個字串
const int MAX = 10; // 每個字串最大長度，包含\0
int main() {
  char str[DAYS][MAX] = {"Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"};
  for (int i = 0; i < DAYS; i++) {
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

### 判斷數字，印出月份英文
{% highlight c++ linenos %}
//12個月
const int MAX_MONTH = 12;
//最長字母September
//9個字元+\0
const int MAX = 10;
int main() {
  char mon_arr[MAX_MONTH][MAX] = {"January","February","March","April","May","June","July","August","September","October","November","December"};
  int month;
  cout << "請輸入數字月份(1~12):";
  cin >> month;
  //陣列索引介於0..11，所以要把month-1
  cout << mon_arr[month-1] << endl;
  return 0;
}
{% endhighlight %}
```
請輸入數字月份(1~12):12
December
```