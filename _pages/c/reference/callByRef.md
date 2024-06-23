---
title: 傳參考
date: 2024-06-20
keywords: c++, call by reference
---

參考文件

[函式參數為指標]({% link _pages/c/pointer/pointerParam.md %})

傳值與傳參考的程式碼幾乎一模一樣，差別只在於，傳值無法修改傳入的參數，傳值的意義在於不要讓函式修改傳入的參數，而傳參考是允許函式存取傳入的參數。

## 宣告

語法

```
函式回傳資料型態 函式名(資料型態& 別名, ...);

void callByRef(int& param1, int& param2);

```

參數宣告為資料型態&，參數就變成別名，[引數]({% link _pages/c/pointer/pointerParam.md %}#引數-argument) 就變成原始變數。


## 傳值

{% highlight c++ linenos %}
void callByValue(int param1, int param2){
    param1 = 100;
    param2 = 200;
}
int main() {
    int i = 10;
    int j = 20;
    callByValue(i,j);
    cout << "callByValue:" << endl;
    cout << "i = " << i << ", j = " << j << endl;
    return 0;
}    
{% endhighlight %}

```
callByValue:
i = 10, j = 20
```

## 傳參考

{% highlight c++ linenos %}
void callByRef(int& param1, int& param2){
    param1 = 100;
    param2 = 200;
}
int main() {
    int i = 10;
    int j = 20;
    callByRef(i,j);
    cout << "callByReference:" << endl;
    cout << "i = " << i << ", j = " << j << endl;
    return 0;
}    
{% endhighlight %}

```
callByReference:
i = 100, j = 200
```

## 傳值與傳參考的差別

二者只有參數不同。

- 傳值
void callByValue(int param1, int param2)

- 傳參考
void callByRef(int**&** param1, int**&** param2)
