---
title: malloc
date: 2025-03-13
keywords: c, malloc
---
## 語法

{% highlight c++ linenos %}
void* malloc(size_t __size)
{% endhighlight %}

## 傳回值型態
由於malloc傳回的類型是void\*指標地址，void\*可以強轉成任何型態。

所以可以[強制轉型][1]
{% highlight c++ linenos %}
char* name = (char* )malloc(10);  // 10byte
int* num = (int* )malloc(1 * 1024 * 1024);  // 1byte * 1024 = 1kb ->1kb * 1024=1mb
{% endhighlight %}

## 參數size_t

參數`size_t  __size`代表設定空間大小

也可以這樣定義
{% highlight c++ linenos %}
// int 4byte * 10 = 40 byte
// 申請40byte的記憶體空間
int* num = (int* )malloc(sizeof(int) * 10);
{% endhighlight %}

變數申請動態空間
{% highlight c++ linenos %}
char* name = (char* )malloc(100);
{% endhighlight %}

### 初始化memset
-[memset][2]
把指標所指向的記憶體內容，初始化要定義的值  
以下是把jj指標所指向的記憶體空間，全變成NULL，0等於NULL  
{% highlight c++ linenos %}
void* jj = malloc(1 * 1024 * 1024);
size_t jj_size = 1 * 1024 * 1024;
memset(jj, 0, jj_size);  // 初始化jj指標所指向的內存為0
{% endhighlight %}

### 釋放記憶體空間
使用完要手動釋放空間，不然會造成記憶體洩露  
{% highlight c++ linenos %}
name = 0;  // 相當於 name = NULL  
free(name);  
name = nullptr;  
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
  // jj指標指向1mb的動態內存空間1byte*1024 = 1kb ->1kb*1024=1mb
  void* jj = malloc(1 * 1024 * 1024);
  size_t jj_size = 1 * 1024 * 1024;
  memset(jj, 0, jj_size);  // 初始化jj指標所指向的記憶體位址的值為0
  free(jj);  // 把jj指標的空間釋放，若使用malloc要用free來釋放空間
  jj = 0;    // jj指標設為0也就是null
{% endhighlight %}

[1]: {% link _pages/c/conversion/type_conversion.md %}
[2]: {% link _pages/c/array/array.md %}#memset陣列清空