---
title: 函式傳值與回傳值
date: 2024-07-02
keywords: c++, call by value
---

Prerequisites:

- [引數][1]

## 函式傳遞值 Call by value

函式會建立新的變數將引數拷貝到新的變數，將新的變數作為參數，所以在函式對參數做的任何操作，實際上是對新的變數做操作，而不是對引數做操作。

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

函式會將傳回值拷貝到暫存器或stack中，然後再將拷貝的值傳回給呼叫函式的呼叫者。

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
{% highlight c++ linenos %}

[1]: {% link _pages/c/pointer/pointerParam.md %}#引數-argument