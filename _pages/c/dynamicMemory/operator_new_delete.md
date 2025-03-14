---
title: operator new delete
date: 2025-03-13
keywords: c++, operator new, operator delete
---

Prerequisites:
- [malloc][2]
- [memset][3]

自己實作new與delete

## 呼叫順序

### new

1. 先呼叫operator new()
2. 呼叫建構子

### delete

1. 呼叫解構子
2. 呼叫operator delete()

## 語法

new的寫法<br>
- operator new
- 傳回值固定void*
- 參數型態固定size_t
- return 指標
- 使用malloc(大小)

new語法<br>
{% highlight c++ linenos %}
void* operator new(size_t size) {
  cout << "全域new size = " << size << endl;
  void* ptr = malloc(size);
  cout << "全域 address = " << ptr << endl;
  return ptr;
}
{% endhighlight %}

delete的寫法<br>
- operator delete
- 傳回值固定void
- 參數型態固定void*
- 使用free(指標)

delete語法<br>
{% highlight c++ linenos %}
void operator delete(void* ptr) {
  if (ptr == nullptr) return;
  cout << "全域 delete address = " << ptr << endl;
  free(ptr);
}
{% endhighlight %}

在全域的位置添加上述new與delete的語法，建立物件就會使用自建的new與delete語法。

## new基本型態
{% highlight c++ linenos %}
void* operator new(size_t size) {
  cout << "全域new size = " << size << endl;
  void* ptr = malloc(size);
  cout << "全域 address = " << ptr << endl;
  return ptr;
}
void operator delete(void* ptr) {
  if (ptr == nullptr) return;
  cout << "全域 delete address = " << ptr << endl;
  free(ptr);
}
int main() {
  int* ptr1 = new int(10);
  cout << "int address = " << (void*)ptr1 << " , value = " << *ptr1 << endl;
  delete ptr1;
  return 0;
}
{% endhighlight %}
```
全域new size = 4
全域 address = 0x600000004050
int address = 0x600000004050 , value = 10
全域 delete address = 0x600000004050
```

## new類別

{% highlight c++ linenos %}
void* operator new(size_t size) {
  cout << "全域new size = " << size << endl;
  void* ptr = malloc(size);
  cout << "全域 address = " << ptr << endl;
  return ptr;
}
void operator delete(void* ptr) {
  if (ptr == nullptr) return;
  cout << "全域 delete address = " << ptr << endl;
  free(ptr);
}
// 建立Student物件
class Student {
  string name;
};
int main() {
  Student* s1 = new Student();
  cout << "student address = " << (void*)s1 << endl;
  delete s1;
  return 0;
}
{% endhighlight %}
```
全域new size = 24
全域 address = 0x600000203140
student address = 0x600000203140
全域 delete address = 0x600000203140
```

## 類別自建operator new與delete

- [靜態類別變數與函式][1]

如果類別自己有operator new與delete，就不會使用全域的new與delete

### 靜態成員函式的new與delete
1. 類別中的operator new與delete是static靜態成員函式
2. 因為是靜態成員函式，所以只能使用靜態成員變數與靜態成員函式，不能使用非靜態成員變數與函式。
3. 靜態成員變數需要初始化

### 程式碼
{% highlight c++ linenos %}
void* operator new(size_t size) {
  cout << "全域new size = " << size << endl;
  void* ptr = malloc(size);
  cout << "全域 address = " << ptr << endl;
  return ptr;
}
void operator delete(void* ptr) {
  if (ptr == nullptr) return;
  cout << "全域 delete address = " << ptr << endl;
  free(ptr);
}
// 建立Student物件
class Student {
 public:
  string name;
  void* operator new(size_t size) {
    cout << "類別中 new size = " << size << endl;
    void* ptr = malloc(size);
    cout << "類別中 address = " << ptr << endl;
    return ptr;
  }
  void operator delete(void* ptr) {
    if (ptr == nullptr) return;
    cout << "類別中 delete address = " << ptr << endl;
    free(ptr);
  }
};
int main() {
  int* ptr1 = new int(10);
  cout << "int address = " << (void*)ptr1 << " , value = " << *ptr1 << endl;
  delete ptr1;  
  Student* s1 = new Student();
  cout << "student address = " << (void*)s1 << endl;
  delete s1;
  return 0;
}
{% endhighlight %}
```
全域new size = 4
全域 address = 0x600000004050
int address = 0x600000004050 , value = 10
全域 delete address = 0x600000004050
類別中 new size = 24
類別中 address = 0x600000203140
student address = 0x600000203140
類別中 delete address = 0x600000203140
```

## 自建pool

Student只包含二個成員變數，分別是int id_與 int age_，大小共8byte。

自建的pool為18byte，把pool分為二個區塊來儲存物件，可以存二個物件，超過pool的大小，就用系統的空間來儲存記憶體位址。

第1個byte作為標誌位，1代表使用，0代表沒使用，第2個byte為第一區塊的資料存放的記憶體位址。

第9個byte作為標誌位，1代表使用，0代表沒使用，第10個byte為第二區塊的資料存放的記憶體位址。

![img]({{site.imgurl}}/other/pool.png)

使用char\*來儲存記憶體位址，因為是char是1byte，一次只會遞增或遞減1byte。

initPool()與freepool()都為靜態函式，使用方式類別::靜態函式()

{% highlight c++ linenos %}
// 建立Student物件
class Student {
 public:
  int id_;
  int age_;
  // char是1byte，一次只會遞增或遞減1byte。
  static char* pool_;  // pool的記憶體開始位址
  static bool initPool() {
    pool_ = (char*)malloc(18);  // 建立18byte大小的記憶體空間
    if (pool_ == nullptr) return false;  // 申請記憶體位址失敗返回
    memset(pool_, 0, 18);  // 把記憶體的值全初始化為0
    cout << "開始位址 = " << (void*)pool_ << endl;
    return true;
  }
  static void freepool() {
    if (pool_ == nullptr) return;  // 記憶體位址是空就return返回
    free(pool_);  // 記憶體釋放
    cout << "記憶體釋放" << endl;
  }
  Student() {
    cout << "建構子" << endl;
  }
  ~Student() {
    cout << "解構子" << endl;
  }
  void* operator new(size_t size) {
    if (pool_[0] == 0) {
      cout << "第一區塊位址 = " << (void*)(pool_ + 1) << endl;
      pool_[0] = 1;  // 註記為已使用
      return pool_ + 1;  // 傳回記憶體位址
    }
    if (pool_[9] == 0) {
      cout << "第二區塊位址 = " << (void*)(pool_ + 10) << endl;
      pool_[9] = 1;  // 註記為已使用
      return pool_ + 10;  // 傳回記憶體位址
    }
    // 以上位址都使用，就用系統內部的記憶體
    void* ptr = malloc(size);
    cout << "系統 address = " << ptr << endl;
    return ptr;
  }
  void operator delete(void* ptr) {
    if (ptr == nullptr) return;
    // 如果參數的位址是第一個區間的位址
    if (ptr == pool_ + 1) {
      cout << "free 第一個區間" << endl;
      pool_[0] = 0;  // 註記為沒在使用
      return;
    }
    // 如果參數的位址是第二個區間的位址
    if (ptr == pool_ + 10) {
      cout << "free 第二個區間" << endl;
      pool_[9] = 0;  // 註記為沒在使用
      return;
    }
    // 如果傳進來的位址不是pool_中的位址，代表是系統的位址，使用free
    free(ptr);
  }
};
// 靜態成員變數要初始化
char* Student::pool_ = nullptr;
int main() {
  if (Student::initPool() == false) {
    cout << "init fail" << endl;
    return -1;
  }
  cout << "========= create s1 ============" << endl;
  Student* s1 = new Student();
  cout << "s1 address = " << s1 << endl;
  cout << "========= create s2 ============" << endl;
  Student* s2 = new Student();
  cout << "s2 address = " << s2 << endl;
  cout << "========= create s3 ============" << endl;
  Student* s3 = new Student();
  cout << "s3 address = " << s3 << endl;
  cout << "========= del s1 ============" << endl;
  delete s1;
  cout << "========= create s4 ============" << endl;
  Student* s4 = new Student();
  cout << "s4 address = " << s4 << endl;
  cout << "========= del s2 ============" << endl;
  delete s2;
  cout << "========= del s3 ============" << endl;
  delete s3;
  cout << "========= del s4 ============" << endl;
  delete s4;
  Student::freepool();
  return 0;
}
{% endhighlight %}
```
開始位址 = 0x600000209300
========= create s1 ============
第一區塊位址 = 0x600000209301
建構子
s1 address = 0x600000209301
========= create s2 ============
第二區塊位址 = 0x60000020930a
建構子
s2 address = 0x60000020930a
========= create s3 ============
系統 address = 0x600000008030
建構子
s3 address = 0x600000008030
========= del s1 ============
解構子
free 第一個區間
========= create s4 ============
第一區塊位址 = 0x600000209301
建構子
s4 address = 0x600000209301
========= del s2 ============
解構子
free 第二個區間
========= del s3 ============
解構子
========= del s4 ============
解構子
free 第一個區間
記憶體釋放
```

[1]: {% link _pages/c/class/static_class.md %}
[2]: {% link _pages/c/dynamicMemory/malloc.md %}
[3]: {% link _pages/c/array/array.md %}#memset陣列清空