---
title: 函式參考
date: 2024-11-01
keywords: c++, function reference
---

Prerequisites:

- [函式指標][1]

函式參考與函式指標相同，不同的只是符號不同，一個用星號`*`，一個用`&`

函式指標宣告
<pre>
void (<span class="markline">*</span>func_pointer)(int, const string&);
</pre>

函式參考宣告
<pre>
void (<span class="markline">&</span>func_ref)(int, const string&);
</pre>

{% highlight c++ linenos %}
//函式宣告
void print(int code, const string& msg);
int main() {
  //宣告函式指標，指到print函式
  void (*func_pointer)(int, const string&) = print;
  //呼叫函式
  func_pointer(500, "Server Error.");
  
  //宣告函式參考，指到print函式
  void (&func_ref)(int, const string&) = print;
  //呼叫函式
  func_ref(500, "Server Error.");
  return 0;
}
//函式定義
void print(int code, const string& msg) {
  cout << "Error code = " << code << " , Msg = " << msg << endl;
}
{% endhighlight %}

```
Error code = 500 , Msg = Server Error.
Error code = 500 , Msg = Server Error.
```

[1]: {% link _pages/c/function/functionPointer.md %}