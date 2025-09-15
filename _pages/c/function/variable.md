---
title: 函式可變參數
date: 2025-09-15
keywords: c++, variable parameter 
---
## 參數
函式第1個參數要有值，不管任何類型都可以。<br>
第2個參數為...，...一定要放函式參數最後面。<br>
```
傳回值 函式名(任何類型 參數名, ...)
void func1(int num, ...)
```

多個參數範例
```
傳回值 函式名(任何類型 參數名1,任何類型 參數名2,任何類型 參數名3, ...)
void func1(char c, int num, double d, ...)
```

## va_list
va_list實際上是一個指標，宣告指向可變參數的指標。
{% highlight c++ linenos %}
va_list v1;
{% endhighlight %}

## va_start
宣告va_list的開始位址。<br>
第2個參數為可變參數`...`的前一個參數名。<br>
{% highlight c++ linenos %}
va_start(v1, num);
{% endhighlight %}

本範例可變參數的前一個參數名為num。<br>
{% highlight c++ linenos %}
void func1(int num, ...) 
{% endhighlight %}

va_list指標移到`...`可變參數的第0個位罝。
```
  前一個  va_list
      ↓  ↓
func1(3, 10, 30, 20);
```

## 印出可變參數指向的值
va_list實際上是一個指標，所以要印出它指向的值，要使用取值運算子\*。<br>
{% highlight c++ linenos %}
void func1(int num, ...) {
  va_list v1;
  va_start(v1, num);
  printf("v1 = %d \n",* v1);
}
int main() {
  func1(3, 10, 30, 20);
  return 0;
}
{% endhighlight %}
```
v1 = 10 
```
## va_arg 指標移動
第1個參數為可變參數指標，第2個參數為傳回值類型。<br>
每呼叫一次va_arg()函式，先傳回va_list的值`*va_list`，然後移動va_list指標\+1，實際上是根據第2個參數知道移動的byte數目，int是4byte，所以記憶體會移動4byte，移到下一個參數位置。<br>
```
傳回值類型 va_arg(v1, 傳回值類型);
```

一開始指向的位址
```
       va_list
         ↓
func1(3, 10, 30, 20);
```

va_list指標\+1
```
           va_list
             ↓
func1(3, 10, 30, 20);
```

完整程式碼
{% highlight c++ linenos %}
void func1(int num, ...) {
  va_list v1;
  va_start(v1, num);
  printf("v1 = %d \n",*v1);
  int val = 0;
  for (int i = 0; i < num; i++) {
    val = va_arg(v1, int);
    printf("%d\n",val);
  }
}
int main() {
  func1(3, 10, 30, 20);
  return 0;
}
{% endhighlight %}
```
v1 = 10 
10
30
20
```