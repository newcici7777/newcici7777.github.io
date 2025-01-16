---
title: 函式傳值與傳回值
date: 2024-07-02
keywords: c++, call by value
---

Prerequisites:

- [引數][1]

## 函式傳遞值 Call by value

### 解釋1

函式會建立新的變數將引數拷貝到新的變數，將新的變數作為參數，所以在函式對參數做的任何操作，實際上是對新的變數做操作，而不是對引數做操作。

### 解釋2

參數一進入函式，函式即建立新的變數保管這些參數值，同時新變數的資料型態完全吻合參數的資料型態，這種傳遞參數的方式會先把參數複製一份，稱為傳值呼叫(call by value)。

### 解釋3 

以傳值方式傳遞參數的函式會建立一個與參數型態相同的變數，然後把參數的值複製到該變數，因此，函式不能存取原來的變數，僅能存取其拷貝值。當函式無須修改原來的變數值時，傳值的方法很實用，同時保證函式不會破壞原來的變數值。

以下程式碼是將引數x傳遞給函式setX()，將引數x拷貝到參數r，對參數r進行修改，並不會影嚮x引數。

{% highlight c++ linenos %}
//r為參數
void setX(int r){
  r = 1000;
}
int main() {
  int x = 10;
  cout << "Before x = " << x << endl;
  //x為引數
  setX(x);
  cout << "After x = " << x << endl;
  return 0;
}
{% endhighlight %}

```
Before x = 10
After x = 10
```

## 函式傳回值是值

### 解釋1
以下的程式碼，main()和getValue()都各別有存放傳回值r的記憶體位址，待getValue()的r變數傳回給main()的r變數後，getValue()的r變數的記憶體位址就會被釋放。

### 解釋2

- [記憶體配置][2]

函式會將傳回值拷貝到暫存器或stack中，然後再傳回拷貝到暫存器或stack中的值，接下來再把函式中區域變數記憶體釋放。

{% highlight c++ linenos %}
int getValue(){
  int r = 1000;
  return r;
}
int main() {
  int r = getValue();
  cout << "r = " << r << endl;
  return 0;
}
{% endhighlight %}

```
r = 1000
```

[1]: {% link _pages/c/basic/param.md %}
[2]: {% link _pages/c/dynamicMemory/memoryLayout.md %}#變數記憶體位址