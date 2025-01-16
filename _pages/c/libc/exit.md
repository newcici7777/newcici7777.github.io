---
title: exit終止程序
date: 2024-12-16
keywords: c++, exit, return
---

程序正常中止的方式如下
- exit(0)
- return 0
- 在thread中，最後一個thread呼叫return()
- 在thread中，最後一個thread呼叫pthread_exit()

## exit

在任意子函式呼叫exit()，就會直接終止程序，不會再返回到main()函式。

以下程式碼

main()呼叫func1()，func1()呼叫func2()，func2()呼叫exit(0)離開程序。程式執行到func2，不會返回main()
{% highlight c++ linenos %}
#include <iostream>
#include <signal.h>
#include <unistd.h>  // system() head file
using namespace std;
void func2() {
  cout << "func2()" << endl;
  exit(0);
}
void func1() {
  cout << "func1()" << endl;
  // call func2
  func2();
  cout << "返回func1()" << endl;
}
int main() {
  cout << "main()" << endl;
  // call fun1
  func1();
  cout << "返回main()" << endl;
  return 0;
}
{% endhighlight %}
```
main()
func1()
func2()
```

若把以上程式碼exit(0)移掉，執行結果如下
```
main()
func1()
func2()
返回func1()
返回main()
```

`exit(0)`與`return 0`，0為預設終止程序傳回值，若不寫，也會傳回0，以下指令印出上一個程序執行傳回的值。
```
echo $?
```

## 終止程序與解構子

### return與解構子

return會呼叫<span class="mark">全域與區域變數</span>的解構子。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
 public:
  Student(const string &name) : name(name) {}
  ~Student() {
    cout << "解構子 : " << name << endl;
  }
 private:
  string name;
};
// 全域變數
Student s1("Mary");
int main() {
  // 區域變數
  Student s2("Bill");
  return 0;
}
{% endhighlight %}
```
解構子 : Bill
解構子 : Mary
```

### exit與解構子

exit只會呼叫<span class="mark">全域</span>的解構子。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
 public:
  Student(const string &name) : name(name) {}
  ~Student() {
    cout << "解構子 : " << name << endl;
  }
 private:
  string name;
};
// 全域變數
Student s1("Mary");
int main() {
  // 區域變數
  Student s2("Bill");
  exit(0);
}
{% endhighlight %}
```
解構子 : Mary
```

### `_exit()`與`_Exit()`與解構子

`_exit()`與`_Exit()`不會呼叫任何解構子

{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
class Student {
 public:
  Student(const string &name) : name(name) {}
  ~Student() {
    cout << "解構子 : " << name << endl;
  }
 private:
  string name;
};
// 全域變數
Student s1("Mary");
int main() {
  // 區域變數
  Student s2("Bill");
  _exit(0);
}
{% endhighlight %}


## atexit()

可使用atexit()函式，註冊終止程序時要呼叫的函式。

語法
```
int atexit(void (*function)(void));
```
參數為函式指標(要符合函式指標格式)
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
void destroy1() {
  cout << "呼叫destroy1" << endl;
}
void destroy2() {
  cout << "呼叫destroy2" << endl;
}
int main() {
  atexit(destroy1);
  atexit(destroy2);
  exit(0);
}
{% endhighlight %}
```
呼叫destroy2
呼叫destroy1
```