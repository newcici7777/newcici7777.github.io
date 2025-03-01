---
title: 程式碼格式
date: 2024-11-15
keywords: c++, format
---

以下內容參考[google C++風格指南](https://www.cclo.idv.tw/google-cpp-styleguide-zhTW/formatting.html#class-format)
## 命名

### 命名規則

名稱要有描述性；少用縮寫。

{% highlight c++ linenos %}
int price_count_reader;  // 無縮寫。
int num_errors;      // "num" 是很常見的縮寫字。
int num_dns_connections;   // 大部份的人都知道 "DNS" 是啥。
int lstm_size;       // "LSTM" 在機器學習領域中是個常用的縮寫字。
{% endhighlight %}

{% highlight c++ linenos %}
int n;           // 毫無意義。
int nerr;          // 模稜兩可的縮寫。
int n_comp_conns;      // 模稜兩可的縮寫。
int wgc_connections;     // 只有貴團隊知道是啥意思。
int pc_reader;       // "pc" 有太多可能的解釋了。
int cstmr_id;        // 有刪減若干字母。
FooBarRequestInfo fbri;  // 根本不是個單字。
{% endhighlight %}

### 檔案名

檔案名稱要全部小寫，可以包含底線 (_) 或減號 (-)。依專案慣例來選用。如果專案沒有一致的用法，用 「_」 比較好。

### 變數名

#### 一般變數、函式參數

變數（包括函式的參數）以及資料成員的名稱一律小寫，單字之間用底線連接。類別的資料成員結尾處多加一個底線（但結構的資料成員不用），如：a_local_variable、a_
struct_data_member、a_class_data_member_。

{% highlight c++ linenos %}
string table_name;  // 可 - 用底線。
string tablename;   // 可 - 全小寫。
{% endhighlight %}

#### 類別成員

小寫變數名，最後加底線_
{% highlight c++ linenos %}
class TableInfo {
  ...
  private:
  string table_name_;  // 可 - 字尾加底線。
  string tablename_;   // 可。
  static Pool<TableInfo>* pool_;  // 可。
};
{% endhighlight %}

#### 結構成員

不用最後加底線_

{% highlight c++ linenos %}
struct UrlTableProperties {
  string name;
  int num_entries;
}
{% endhighlight %}

#### 命名空間

命名空間用小寫字母命名，避免使用不當的縮寫。

把檔案名稱加到名稱中，可以有效建立獨一無二的名稱（例如 在 frobber.h 中，就用 websearch::index::frobber_internal 這樣的名稱）。

#### 函式名

一般函式使用大小寫混合。

一般來說，函式名稱的第一個字母要大寫，其後每個單字的字首字母均大寫。

{% highlight c++ linenos %}
AddTableEntry()
DeleteUrl()
OpenFileOrDie()
{% endhighlight %}

#### 類別、結構、型別別名、列舉，以及模板型別參數

字首大寫，不能有底線，採駝峰式命名

型別名稱的每個單字首字母均大寫，不使用底線：MyExcitingClass，MyExcitingEnum。

所有型別命名（類別、結構、型別別名、列舉，以及模板型別參數）均使用相同規則。型別名稱的第一個字母要大寫，其後每個單字的字首字母均大寫，例如:

{% highlight c++ linenos %}
// 類別和結構
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// typedefs
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// using 別名
using PropertiesMap = hash_map<UrlTableProperties *, string>;

// 列舉
enum UrlTableErrors { ...
{% endhighlight %}

#### 常數

宣告時加上 constexpr 或 const，且整個程式執行時間內都不會改變的變數，命名時需以 「k」 開頭，後面的字母以混合大小寫的方式書寫。在少數大寫字無法將單字隔開的情況下，可以使用底線當作區隔。舉例來說：

{% highlight c++ linenos %}
const int kDaysInAWeek = 7;
const int kAndroid8_0_0 = 24;  // Android 8.0.0
{% endhighlight %}

#### 列舉

- 列舉若是常數，按照常數標準。
- 列舉若是型別，按照類別標準。

## 註解

使用 // 或 /* */，只要一致就好。

註解必須清晰易讀且平舖直述。

### 類別

類別前都要附帶一份註解(除非看到類別名就知道是在做什麼)，描述類別的功能和用法，了解如何使用、何時該使用這個類別。

類別運作以及實作方法的註解應該要放在類別成員函式的實作之處

### 函式

函式前加上用法說明的註解，除非看到函式名就知道是在做什麼，就可以不用寫。

### 變數名

通常變數名本身足以很好說明變數的用途。某些情況下，還是需要額外的註解說明。

### 全域變數

所有的全域變數都要註解說明含義、用途，以及為什麼要將它宣告為全域變數

### TODO

TODO 註解要使用全大寫的字串 TODO

在那些臨時的、短期的解決方案，或已經夠好但仍不完美的程式碼旁加上 TODO 註解

{% highlight c++ linenos %}
// TODO(kl@gmail.com): Use a "*" here for concatenation operator.
// TODO(Zeke) change this to use relations.
// TODO(bug 12345): remove the "Last visitors" feature
{% endhighlight %}

### 棄用deprecation

若已被棄用，用全大寫 DEPRECATED 註解標記。

## 空白

每一句程式碼內縮2個空白

以下程式碼vector前面留有2個空白

{% highlight c++ linenos %}
int main() {
  vector<int> v = {1, 2, 3, 4, 5};
  return 0;
}
{% endhighlight %}

xcode把tab改為2個空白

![img]({{site.imgurl}}/xcode_c++/tab_space.png)

### if

注意在所有情況下，if 和左括號間都有個空格。如果有大括號的話，右括號和左大括號之間也要有個空格：

#### 正確方式

{% highlight c++ linenos %}
if (condition) {  // 可 - IF 後面和 { 前面都留有適當的空格。
{% endhighlight %}

#### 錯誤方式
{% highlight c++ linenos %}
if (condition)   // 差 - IF 後面沒空格。
if (condition){   // 差 - { 前面沒空格。
if (condition){  // 前面兩項錯誤犯好犯滿。
{% endhighlight %}

#### if,else if,else

正確方式

{% highlight c++ linenos %}
if (condition) {  // 括號裡沒空格。
  ...  // 2 空格縮排。
} else if (...) {  // else 與 if 的右大括號放在同一行。
  ...
} else {
  ...
}
{% endhighlight %}

簡短的條件語句可以寫在同一行，如果這樣可讀性比較高的話。只有當句子簡單並且沒有使用 else 子句時可以使用：
{% highlight c++ linenos %}
if (x == kFoo) return new Foo();
if (x == kBar) return new Bar();
{% endhighlight %}

如果述句中有 else 的話就禁止如此使用：

{% highlight c++ linenos %}
// 不可以這樣子 - 當 ELSE 子句存在時，IF 陳述句卻只擠在同一行
if (x) DoThis();
else DoThat();
{% endhighlight %}

#### if大括號

一般來說，單行語句不需要使用大括號，如果你喜歡用也沒問題

{% highlight c++ linenos %}
if (condition)
  DoSomething();  // 2 空格縮排。

if (condition) {
  DoSomething();  // 2 空格縮排。
}
{% endhighlight %}

但如果整個述句中某個 if-else 的區塊使用了大括號的話，其它區塊也必須使用：

{% highlight c++ linenos %}
// 不可以這樣子 - IF 有大括號 ELSE 卻沒有。
if (condition) {
  foo;
} else
  bar;

// 不可以這樣子 - ELSE 有大括號 IF 卻沒有。
if (condition)
  foo;
else {
  bar;
}
{% endhighlight %}

正確方式

{% highlight c++ linenos %}
// 只要其中一個區塊用了大括號，兩個區塊都要用。
if (condition) {
  foo;
} else {
  bar;
}
{% endhighlight %}

### for

for () {....}中的內縮仍是2個空白

{% highlight c++ linenos %}
int main() {
  vector<int> v = {1, 2, 3, 4, 5};
  for (auto it = v.crbegin(); it != v.crend(); it++) {
  //內縮2個空白
  cout << *it << ", ";
  }
  return 0;
}
{% endhighlight %}


for與圓括號()之間要有空白，圓括號()與左大括號{要有空白，並且左大括號{與for在同一行
{% highlight c++ linenos %}
  for () {	
  }
{% endhighlight %}


圓括號()與裡面的條件，開頭與結尾是沒空白
{% highlight c++ linenos %}
  for (int i = 0; i < 10; i++) {	
  }
{% endhighlight %}


分號;後留一個空白，=等於的前後留空白，<的前後留空白，++前後不用留空白
{% highlight c++ linenos %}
  for (int i = 0; i < 10; i++)
{% endhighlight %}

分號後一定要有空格。
{% highlight c++ linenos %}
// 迴圈中，分號後一定要有空格。
  for (auto it = first; ; ) 
{% endhighlight %}

### 引數

大括號{...}裡面的值，逗號(,)右側留一個空白，左側不留空白，大括號{...}中，前後不留空白。

變數名v與類型留一個空白

=等於，前後留空白

{% highlight c++ linenos %}
  vector<int> v = {1, 2, 3, 4, 5};
{% endhighlight %}

### 類別

- public前面留一個空白
- public裡面的述敘與public是一個空白

{% highlight c++ linenos %}
class callbackObj {
 public://與最邊緣留1個空白
  //與public留1個空白，與最邊緣留2個空白
  void print(const string& msg) {
  //與void留2個空白，與最邊緣留4個空白
  cout << msg << endl;
  }
};
{% endhighlight %}

### 模板

template與尖括號\<，中間有空白

{% highlight c++ linenos %}
template <typename T, typename U>
{% endhighlight %}

### 註解空白與對齊

#### 2個斜線後面加一個空白

{% highlight c++ linenos %}
// Process "element" unless it was already processed.
auto iter = std::find(v.begin(), v.end(), element);
{% endhighlight %}

#### 行尾註解空2個空白

在行尾加兩格空隔後，加上2個斜線，再加一個空白後，開始註解
{% highlight c++ linenos %}
if (.....)
  return;  // Error already logged.
{% endhighlight %}

#### 註解對齊

{% highlight c++ linenos %}
DoSomething();          // 把註解放這裡才能和下一行對齊。
DoSomethingElseThatIsLonger();  // 註解和程式碼之間要有兩個空格。  
{ // 當開啟一個新的作用域時，可以只放一個空隔，
  // 這樣接下來的註解和程式碼都可以和前面那行對齊。
  DoSomethingElse();  // 一般來說行註解前面都需要兩個空隔。
}
std::vector<string> list{
          // 在條列初始化中，用來說明下一個元素的註解...
          "First item",
          // .. 必須要妥善對齊。
          "Second item"};
{% endhighlight %}

### define

以井號 # 開頭的前置處理器指令一律從一行的最開頭寫起。

{% highlight c++ linenos %}
// 可 - 指令從行首寫起
  if (lopsided_score) {
#if DISASTER_PENDING      // 正確 -- 從行首寫起。
    DropEverything();
# if NOTIFY               // 可以，但非必要 -- # 後面有空格
    NotifyClient();
# endif
#endif
    BackToNormal();
  }
{% endhighlight %}

## 斷行

除非必要，不要使用斷行。尤其是：兩個函式定義之間的斷行不要超過 2 行，函式起始處不要是斷行，最後一行也不要是斷行，其餘地方也儘量少用斷行。在一個程式碼區塊中，斷行像是文章中的段落：在視覺上將兩個想法區隔開來。

- 函式內開頭或結尾的斷行對可讀性沒有幫助。
- 在多重 if-else 區塊裡加斷行對可讀性可能有些幫助。
- 在註解前面加空行通常可以增加可讀性，引入一段新的註解等於在介紹一個新想法的開始，此時加上空行可以清楚地表示這段註解是在說明接下來的程式碼，而非延續前面的行為。

## initializer_list

若是你不得不斷行，放在下一行，加上4格的縮排

{% highlight c++ linenos %}
// 若是你不得不斷行。
SomeFunction(
  {"assume a zero-length name before {"},
  some_other_function_parameter);
SomeType variable{
  "This is too long to fit all in one line"};
MyType m = {  // 你也可以在 { 前斷行。
  superlongvariablename1,
  superlongvariablename2,
  {short, interior, list},
  {interiorwrappinglist,
   interiorwrappinglist2}};
{% endhighlight %}

## 關係運算子放在行尾

若是你不得不斷行，放在下一行，加上4格的縮排

關係運算子斷行時一律放在行尾

{% highlight c++ linenos %}
string time_str =
  to_string(tmnow.tm_year + 1900) + "/" +
  to_string(tmnow.tm_mon + 1) + "/" +
  to_string(tmnow.tm_mday) + "/" +
  to_string(tmnow.tm_hour) + ":" +
  to_string(tmnow.tm_min) + ":" +
  to_string(tmnow.tm_sec);
{% endhighlight %}

{% highlight c++ linenos %}
if (this_one_thing > this_other_thing &&
  a_third_thing == a_fourth_thing &&
  yet_another & last_one) {
  ...
}
{% endhighlight %}

## 建構子初始值

建構式初值列可以放在同一行，或換行後縮排 4 個空格。
{% highlight c++ linenos %}
// 當一行可以塞得下時：
MyClass::MyClass(int var) : some_var_(var) {
  DoSomething();
}

// 如果一行塞不下建構式名稱列和初值列的話，你必須
// 在分號前換行，並且縮排 4 個空格
MyClass::MyClass(int var)
  : some_var_(var), some_other_var_(var + 1) {
  DoSomething();
}

// 若是初值列得分成好幾行的話，每個成員各占一行，
// 排列整齊：
MyClass::MyClass(int var)
  : some_var_(var),       // 4 格縮排
    some_other_var_(var + 1) {  // 對齊前一行
  DoSomething();
}

// 和其他程式碼區塊一樣，如果塞得下的話，右大括號可以
// 和左大括號放在同一行。
MyClass::MyClass(int var)
  : some_var_(var) {}
{% endhighlight %}

## 指標與reference

- 在存取成員時，句點或箭頭前後沒有空格。
- 指標運算子 * 或 & 後面沒有空格。

{% highlight c++ linenos %}
x = *p;
p = &x;
x = r.y;
x = r->y;
{% endhighlight %}

{% highlight c++ linenos %}
// 沒問題，空格放在星號前。
char *c;
const string &str;
// 沒問題，空格放在星號後。
char* c;
const string& str;
{% endhighlight %}

允許（但不常用）在同一行宣告式中宣告 1 個以上的變數，但其中不得有指標或是 reference 的宣告，因為這樣的宣告式很容易造成混淆。
{% highlight c++ linenos %}
// 如果對可讀性有幫助就沒問題。
int x, y;
{% endhighlight %}

{% highlight c++ linenos %}
int x, *y;  // 禁止 - 多個變數的宣告式中不得有 & 或 *
char * c;  // 不好 - 星號前後都有空格
const string & str;  // 不好 - & 前後都有空格
{% endhighlight %}

## 類別

存取控制區塊的宣告依次序是 public:、protected:、private:，每次縮排 1 個空格。

- 關鍵詞 public:、protected: 和 private: 要縮排 1 個空格。
- 這些關鍵詞後不要保留空行
- public 放在最前面，然後是 protected，最後是 private。
- 繼承關係，:冒號前後有空白，[建構子初始](#建構子初始值)，也是冒號前後有空白

{% highlight c++ linenos %}
class MyClass : public OtherClass
{% endhighlight %}


{% highlight c++ linenos %}
class MyClass : public OtherClass {
 public:      // 注意有 1 空格縮排！
  MyClass();  // 一般的 2 空格縮排。
  explicit MyClass(int var);
  ~MyClass() {}

  void SomeFunction();
  void SomeFunctionThatDoesNothing() {
  }

  void set_some_var(int var) { some_var_ = var; }
  int some_var() const { return some_var_; }

 private:
  bool SomeInternalFunction();

  int some_var_;
  int some_other_var_;
};
{% endhighlight %}