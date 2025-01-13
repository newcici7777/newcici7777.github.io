---
title: string
date: 2024-11-12
keywords: c++, string
---
Prerequisites:
- [ArrayList實作][1]
- [ArrayList記憶體擴充][2]

## string物件

- 動態分配記憶體位址存放一組字元
- 自動建立記憶體位址與釋放記憶體位址，使用時程式設計師不用做釋放記憶體的動作
- 自動擴展容量
- 底層以ArrayList實作

## string記憶體擴充

string底層是ArrayList實作。

以下程式碼證明string記憶體擴充後，capacity(最大容量)會呈倍數成長。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  //建立空的字串，呼叫string()空的建構子
  string s1;
  //印出string
  cout << "s1 = " << s1 << endl;
  //容量
  cout << "s1.capacity() = " << s1.capacity() << endl;
  //實際用了多少
  cout << "s1.size()" << s1.size() << endl;
  //s1的記憶體位址
  cout << "s1記憶體位址 = " << (void*)s1.c_str() << endl;
  cout << "----------------------------" << endl;
  //記憶體擴展
  //25個字元(Hello 5字元*5遍)
  s1 = "HelloHelloHelloHelloHello";
  //印出string
  cout << "s1 = " << s1 << endl;
  //容量
  cout << "s1.capacity() = " << s1.capacity() << endl;
  //實際用了多少
  cout << "s1.size()" << s1.size() << endl;
  //s1的記憶體位址
  cout << "s1記憶體位址 = " << (void*)s1.c_str() << endl;
  return 0;
}
{% endhighlight %}

```
s1 = 
s1.capacity() = 22
s1.size()0
s1記憶體位址 = 0x7ff7bfeff451
----------------------------
s1 = HelloHelloHelloHelloHello
s1.capacity() = 47
s1.size()25
s1記憶體位址 = 0x600000c00120
```
從執行結果可以看出
- capacity(最大容量)呈倍數成長，從22變成47。
- 記憶位址不同，從0x7ff7bfeff451變成0x600000c00120

	建立一個容量是47的記憶體空間，把s1的值從容量22的記憶體空間搬到容量47的記憶體空間，搬完後把容量22的記憶體空間進行記憶體釋放，所以記憶體位址才會不同。

## string建構子為c字串

### 語法

建構子參數為c字串

```
string(const char *s) 
```
### 呼叫建構子

#### 方式1

```
string s2("Hello world!");
```

#### 方式2

- [建構子參數只有一個，可使用指派運算子][3]

```
string s3 = "Hello World!";
```

#### 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
	// 方式1
  string s2("Hello world!");
  cout << "s2 = " << s2 << endl;
  // 方式2
  string s3 = "Hello World!";
  cout << "s3 = " << s3 << endl;
}
{% endhighlight %}

```
s2 = Hello world!
s3 = Hello World!
```

## string拷貝函式

拷貝函式用於從另一個已經存在的string物件來初始化新物件。

語法

```
string(const string &str)
```

### 方式1

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  string s2("Hello world!");
  cout << "s2 = " << s2 << endl;
  string s3 = s2;
  cout << "s3 = " << s3 << endl;
}
{% endhighlight %}

```
s2 = Hello world!
s3 = Hello world!
```

### 方式2

{% highlight c++ linenos %}
int main() {
  string s1 = "Hello world";
  string s2(s1);
  cout << "s2 = " << s2 << endl;
  cout << "s2 size = " << s2.size() << endl;
  return 0;
}
{% endhighlight %}
```
s2 = Hello world
s2 size = 11
```

## string assign operator=()

物件已經存在的情況下，將一個新值賦給該物件，會啟動assign operator=()

{% highlight c++ linenos %}
string s3;
s3 = "Hello World!";
{% endhighlight %}

## 字串連接

### operator+=
```
// string
string& operator+= (const string& str);
// char*
string& operator+= (const char* s);
// character
string& operator+= (char c);
// initializer list
string& operator+= (initializer_list<charT> il);
```

{% highlight c++ linenos %}
#include <iostream>
#include <cstring>
using namespace std;
int main() {
  string s1 = "Hello";
  string s2 = "hi";
  // const char*
  s1 += " Mary!";
  // string
  s1 += s2;
  // character
  s1 += 'A';
  cout << "s1 = " << s1 << endl;
  return 0;
}
{% endhighlight %}
```
s1 = Hello Mary!hiA
```
### append
```
// char*
string &append(const char *s);
// string
string &append(const string &str);
```

{% highlight c++ linenos %}
int main() {
  string s1 = "Hello";
  // char*
  s1.append(" world!");
  string s2 = " Hi, Mary!";
  // string
  s1.append(s2);
  cout << "s1 = " << s1 << endl;
  return 0;
}
{% endhighlight %}
```
s1 = Hello world! Hi, Mary!
```

## 建構子參數為c字串與n個字元

### n小於c字串的長度

目的

從c字串的位址開始複製n個字元，建立字串。

語法

- 參數1為c字串
- 數數2為複製n個字元

```
string(const char *s,size_t n);
```

完整程式碼
{% highlight c++ linenos %}
int main() {
	//從c字串的位址開始複製5個字元，建立字串
  string s("hello world", 5);
  cout << "s = " << s << endl;
  cout << "size = " << s.size() << endl;
  return 0;
}
{% endhighlight %}
```
s = hello
size = 5
```
### n大於c字串的長度

從c字串的位址開始複製100個字元，建立字串。

從執行結果可以發現，並不會遇到`\0`結尾字元而停止複製，s物件的實際大小也是20字元。

{% highlight c++ linenos %}
int main() {
	//從c字串的位址開始複製20個字元，建立字串
  string s("hello world", 20);
  cout << "s = " << s << endl;
  cout << "size = " << s.size() << endl;
  return 0;
}
{% endhighlight %}
```
s = hello world = siz
size = 20

```

## 參數為string物件與拷貝開始與結束位置

以下是string的拷貝函式，拷貝開始位置(預設從0開始)與結束位置(預設unsigned int最大值)

```
string(const string & str, size_t pos = 0, size_t n = npos)
```
- 參數1 string物件。
- 參數2 拷貝開始位置(預設從0開始)，可不寫。
- 參數3 拷貝結束位置(預設unsigned int最大值)，可不寫。


### 拷貝結束位置介於字元大小間
{% highlight c++ linenos %}
int main() {
  string s1 = "Hello world";
  string s2(s1,0,3);
  cout << "s2 = " << s2 << endl;
  cout << "s2 size = " << s2.size() << endl;
  return 0;
}
{% endhighlight %}
```
s2 = Hel
s2 size = 3
```
### 拷貝結束位置超出字元大小

從執行結果可以發現遇到`\0`結尾字元而停止複製，size仍是11，而不是100。

{% highlight c++ linenos %}
int main() {
  string s1 = "Hello world";
  string s2(s1,0,100);
  cout << "s2 = " << s2 << endl;
  cout << "s2 size = " << s2.size() << endl;
  return 0;
}
{% endhighlight %}
```
s2 = Hello world
s2 size = 11
```

## 建立多個相同字元的string

```
string(size_t n, char c)
```
- 參數1個數
- 參數2字元

{% highlight c++ linenos %}
int main() {
  string s1(100,'C');
  cout << "s1 = " << s1 << endl;
  cout << "s1 size = " << s1.size() << endl;
  return 0;
}
{% endhighlight %}
```
s1 = CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC
s1 size = 100
```

## string成員函式

### 取得string物件記憶體大小
```
size_t capacity() const;
```
### 取得string物件實際使用大小
以下二者相同

```
size_t length() const;
```
```
size_t size() const;
```
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  string s1 = "123566";
  cout << "s1 length = " << s1.length() << endl;
  cout << "s1 size = " << s1.size() << endl;
  return 0;
}
{% endhighlight %}
```
s1 length = 6
s1 size = 6
```

- [strlen()][5]
- [c_str()][6]

strlen()是用於c字元陣列，並非string，使用strlen，需要把string轉成字元陣列指標
```
size_t strlen ( const char * str );
```
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  string s1 = "123566";
  cout << "s1 length = " << s1.length() << endl;
  cout << "s1 size = " << s1.size() << endl;
  cout << "s1 strlen = " << strlen(s1.c_str()) << endl;
  return 0;
}
{% endhighlight %}
```
s1 length = 6
s1 size = 6
s1 strlen = 6
```

### 判斷實際使用的大小為0

若為0返回true
```
bool empty() const;
```
### 清空字串

```
void clear();
```

### 讀取string位置

使用[位置]，可以取得某個位置字元

在 C++ 中，std::string 本質上是一個用於操作字元的容器，內部存儲的字符是一個連續的 char 陣列。

通過 s1[0]，你可以訪問 std::string 中存儲的第一個字元。

&s1[0] 的含義:
- s1[0] 返回的是第一個字元，型別是 char。
- &s1[0] 取得的是該字符在內存中的地址，因此它的型別是 char*。

```
char &operator[](size_t n); 
```

#### 取值

{% highlight c++ linenos %}
  string s1 = "Hello";
  cout << "s1[1] = " << s1[1] << endl;
{% endhighlight %}
```
s1[1] = e
```
#### 取出位址
- [(void\*)印出16進制的位址][4]

使用以下此法，傳回的是char\*，是沒有const
{% highlight c++ linenos %}
  string s1 = "Hello";
  cout << "s1 address = " << (void*)&s1[0] << endl;
{% endhighlight %}
```
s1 address = 0x7ff7bfeff2d1
```

### 取得string物件存放字串的記憶體位址
- [(void\*)印出16進制的位址][4]

&是取得物件的位址

以下是取得string物件中動態分配記憶體空間存放字串的位址，注意！傳回的是const char\*，是有const

```
const char *c_str() const;
```

```
const char *data() const; 
```

{% highlight c++ linenos %}
int main() {
  string s1 = "Hello";
  //取得string物件的記憶體位址
  cout << "s1 & address = " << &s1 << endl;
  //取得string物件中動態分配記憶體空間存放字串的位址
  cout << "s1 c_str address = " << (void*)s1.c_str() << endl;
  cout << "s1 data address = " << (void*)s1.data() << endl;
  return 0;
}
{% endhighlight %}
```
s1 & address = 0x7ff7bfeff450
s1 c_str address = 0x7ff7bfeff451
s1 data address = 0x7ff7bfeff451
```

### 字串作為函式參數

參數語法
```
const string& str
```

以下函式參數的寫法，不僅可以接收c字串指標，也可以接收string類別

若函式參數為const char \*，參數就只能接收c字串指標，string類別要再透過c_str()函式轉成c字串指標。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func1(const string& str) {
  cout << str << endl;
}
int main() {
  char * s1 = "http://www.google.com";
  //可以接受c字串指標
  func1(s1);
  string s2 = "被討厭的勇氣";
  //可以接受字串
  func1(s2);
  return 0;
}
{% endhighlight %}
```
http://www.google.com
被討厭的勇氣
```

### 字串截取

```
string substr(size_t pos = 0,size_t n = npos) const;
```
{% highlight c++ linenos %}
int main() {
  string s1 = "Hello";
  cout << "s1 = " << s1.substr(1,3) << endl;
  return 0;
}
{% endhighlight %}

```
s1 = ell
```

### 字串比較

```
bool operator==(const string &str1,const string &str2) const;
```

{% highlight c++ linenos %}
int main() {
  string s1 = "Hello";
  string s2 = "world";
  cout << "compare result = " << (s1 == s2) << endl;
  return 0;
}
{% endhighlight %}

### 清空

{% highlight c++ linenos %}
  string name = "Bill";
  cout << "name: " << name << endl;
  name.clear();
  cout << "name: " << name << endl;
{% endhighlight %}

```
name: Bill
name: 
```

### resize()

若要對string記憶體位址操作，手動操作會使string本身的動態記憶體分配失效，要手動重新分配它的大小。
{% highlight c++ linenos %}
bool recv(string &buffer, const size_t maxlen) {
  buffer.clear();
  buffer.resize(maxlen);
  // 如果接收成功，recv()會傳回收到資料的大小，-1代表接收失敗，0代表socket斷線，大於0代表成功
  int readn = ::recv(client_fd, &buffer[0], buffer.size(), 0);
  if (readn <= 0) return false;
  // recv()會傳回收到資料的大小，重置大小
  buffer.resize(readn);
  return true;
}
{% endhighlight %}

## string陣列

判斷數字，印出月份英文

{% highlight c++ linenos %}
//12個月
const int MAX_MONTH = 12;
int main() {
  string mon_arr[MAX_MONTH] =
    {"January", "February", "March", "April", "May", "June", 
     "July","August","September","October","November","December"};
  int month;
  cout << "請輸入數字月份(1~12):";
  cin >> month;
  //陣列索引介於0..11，所以要把month-1
  cout << mon_arr[month-1] << endl;
  return 0;
}
{% endhighlight %}

```
請輸入數字月份(1~12):1
January
```

其它[更多成員函式](https://cplusplus.com/reference/string/string/)

[1]: {% link _pages/c/dataStruct/arrayList.md %}
[2]: {% link _pages/c/dataStruct/arrayListExtCap.md %}
[3]: {% link _pages/c/class/constructor.md %}#建構子參數只有一個，可使用指派運算子
[4]: {% link _pages/c/pointer/pointerVoid.md %}
[5]: {% link _pages/c/array/charArray.md %}#字串長度-strlen
[6]: {% link _pages/c/string/string_convert.md %}