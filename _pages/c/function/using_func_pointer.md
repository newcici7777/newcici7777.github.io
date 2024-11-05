---
title: using函式指標別名
date: 2024-10-31
keywords: c++, Using and function pointer
---

Prerequisites:

- [typedef類型別名][1]
- [函式宣告與定義][2]
- [typedef函式指標類型別名][3]
- [函式參考][4]

以上文章必須看完，才能往下看。


## using替函式指標取類型別名

與typedef一樣，都是替函式指標取類型別名

### 定義函式指標類型別名語法

```
using 類型別名 = 函式指標
using Func = void(int, const string&);
```

### 函式宣告

print函式與void(int, const string&)是相符

注意！以下的print後面是沒有()括號

```
Func print;
```

以上程式碼等同普通函式宣告

```
void print(int, const string&);
```

### 函式定義

所謂的函式定義就是有實作程式碼的函式

{% highlight c++ linenos %}
//Func類型別名
using Func = void(int, const string&);
//函式宣告
Func print;
int main() {
    return 0;
}
//函式定義
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
{% endhighlight %}
```
Error code = 500 , Msg = Server error.
```

### 函式指標

宣告函式指標，並指向函式
```
類型別名* 函式指標變數名 = 函式;
Func* func_pointer = print;
```

呼叫函式
```
函式指標變數名(參數)
func_pointer(500, "Server error.");
```

{% highlight c++ linenos %}
//Func函式類型別名
using Func = void(int, const string&);
//函式宣告
Func print;
int main() {
    //宣告函式指標，指向print函式
    Func* func_pointer = print;
    //呼叫函式
    func_pointer(500, "Server error.");
    
    //宣告函式參考，指向print函式
    Func* func_ref = print;
    func_ref(400, "Not Found.");
    return 0;
}
//函式定義
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
{% endhighlight %}
```
Error code = 400 , Msg = Not Found.
```

### 函式參考

宣告函式參考，並指向函式
```
類型別名& 函式指標變數名 = 函式;
Func& func_ref = print;
```

呼叫函式
```
函式參考變數名(參數)
func_ref(500, "Server error.");
```

{% highlight c++ linenos %}
//Func函式類型別名
using Func = void(int, const string&);
//函式宣告
Func print;
int main() {
    //宣告函式參考，指向print函式
    Func* func_ref = print;
    //呼叫函式
    func_ref(400, "Not Found.");
    return 0;
}
//函式定義
void print(int code, const string& msg) {
    cout << "Error code = " << code << " , Msg = " << msg << endl;
}
{% endhighlight %}
```
Error code = 400 , Msg = Not Found.
```

### 類別靜態函式指標

- [靜態類別函式][5]


#### 函式指標語法

加上類別名字以及範圍運算子::
<pre>
void (*func_pointer)(int, const string&) = <span class="markline">Student::</span>print;
</pre>


{% highlight c++ linenos %}
class Student {
public:
    static void print(int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    }
};
int main() {
    //宣告函式指標
    void (*func_pointer)(int, const string&) = Student::print;
    func_pointer(500, "Server error.");
    return 0;
}
{% endhighlight %}
```
Error code = 500 , Msg = Server error.
```

#### 使用using函式指標別名

加上類別名字以及範圍運算子::
<pre>
Func *func_p = <span class="markline">Student::</span>print;
</pre>

{% highlight c++ linenos %}
class Student {
public:
    static void print(int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    }
};
//Func函式類型別名
using Func = void(int, const string&);
int main() {
    //函式類型別名宣告函式指標
    Func *func_p = Student::print;
    func_p(404, "Page not Found.");
    return 0;
}
{% endhighlight %}
```
Error code = 404 , Msg = Page not Found.
```

### 物件成員函式指標

#### 物件成員函式指標語法

- 必須把記憶體位址傳進去，所以有使用&取位址運算子
- 呼叫函式時，使用物件加點(.)運算子與指標運算子(\*)
<pre>
void(<span class="markline">Student::</span>* func_pointer)(int, const string&) = <span class="markline">&Student::</span>print;
    <span class="markline">(student.*func_pointer)</span>(500, "Server error.");
</pre>

{% highlight c++ linenos %}
class Student {
public:
    void print(int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    }
};
int main() {
    //物件宣告
    Student student;
    //成員函式轉成函式指標
    void(Student::* func_pointer)(int, const string&) = &Student::print;
    //函式指標呼叫函式
    (student.*func_pointer)(500, "Server error.");
    return 0;
}
{% endhighlight %}
```
Error code = 500 , Msg = Server error.
```

#### 使用using函式指標別名

1. 函式指標類型別名
	<pre>
	using Func = void<span class="markline">(Student::*)</span>(int, const string&);
	</pre>
2. 建立函式指標
	<pre>
	Func func_pointer = <span class="markline">&Student::</span>print;
	</pre>
3. 呼叫函式指標
	<pre>
	<span class="markline">(student.*func_pointer)</span>(400,"Page not found.");
	</pre>

完整程式碼
{% highlight c++ linenos %}
class Student {
public:
    void print(int code, const string& msg) {
        cout << "Error code = " << code << " , Msg = " << msg << endl;
    }
};
//Func函式類型別名
using Func = void(Student::*)(int, const string&);
int main() {
    //建立物件
    Student student;
    //建立函式指標
    Func func_pointer = &Student::print;
    //呼叫函式指標
    (student.*func_pointer)(400,"Page not found.");
    return 0;
}
{% endhighlight %}
```
Error code = 400 , Msg = Page not found.
```

[1]: {% link _pages/c/basic/typedef.md %}
[2]: {% link _pages/c/function/func_def.md %}
[3]: {% link _pages/c/function/functionPointer.md %}#typedef函式指標類型別名
[4]: {% link _pages/c/function/func_ref.md %}
[5]: {% link _pages/c/class/static_class.md %}