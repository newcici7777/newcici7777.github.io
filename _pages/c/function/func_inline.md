---
title: 內嵌函式
date: 2024-10-14
keywords: c++, function inline
---

## 內嵌函式inline

函式會占記憶體空間，主程式呼叫函式時會跳到函式占用的記憶體位址，待函式結尾再跳回主程式，為了節省跳來跳去的執行時間，可在函式前面加上inline，編譯器一看到inline就會把函式拷貝一份插入在主程式之中。

在以下程式碼print()最前面加上inline，使print()函式變成內嵌函式。

{% highlight c++ linenos %}
inline void print(string s) {
    cout << s << endl;
}
int main() {
    print("test");
    print("abcdef");
    print("aaaa");
    return 0;
}    
{% endhighlight %}

轉變內嵌函式，程式在執行時就會變成以下這樣

{% highlight c++ linenos %}
int main() {
    {
        string s = "test";
        cout << s << endl;
    }
    {
        string s = "abcdef";
        cout << s << endl;
    }
    {
        string s = "aaaa";
        cout << s << endl;
    }
    return 0;
}    
{% endhighlight %}

若是呼叫print() 1000次就會copy程式碼1000份在主程式中，占用記憶體空間。(變數也是占記憶體位址)