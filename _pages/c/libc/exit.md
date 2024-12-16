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