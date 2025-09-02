---
title: 陣列與函式
date: 2025-09-02
keywords: c++, array, function
---
## 陣列名
陣列的記憶體大小是連續
{% highlight c++ linenos %}
int array[5] = {10, 20, 30, 40, 50};
{% endhighlight %}

因為int是4byte記憶體大小，所以每一格記憶體位址都是4byte的差距。
```
位址      內容
0x1000    10   ← array[0]
0x1004    20   ← array[1]
0x1008    30   ← array[2]
0x100C    40   ← array[3]
0x1010    50   ← array[4]
```

在main()函式中的陣列名存放的是array[0]的記憶體位址
```
位址     內容
0x1000  0x1000   ← array, &array, &array[0]
```

以下的程式，main()函式中，印出arr、&arr、&arr[0]，都是印出array[0]的記憶體位址。
{% highlight c++ linenos %}
int main() {
  int arr[3];
  printf("main arr= %p \n", arr);
  printf("main &arr= %p \n", &arr);
  printf("[0]= %p \n", &arr[0]);
  printf("[1]= %p \n", &arr[1]);
  printf("[2]= %p \n", &arr[2]);
  return 0;
}
{% endhighlight %}
```
main arr= 0x7ff7bfeff2dc 
main &arr= 0x7ff7bfeff2dc 
[0]= 0x7ff7bfeff2dc 
[1]= 0x7ff7bfeff2e0 
[2]= 0x7ff7bfeff2e4 
```

## 陣列與函式
### 函式參數
方式1，參數是指標。
{% highlight c++ linenos %}
void func1(int* arr) {
  printf("func1 &arr= %p \n", &arr);
  printf("func1 arr= %p \n", arr);
}
{% endhighlight %}

方式2，參數是陣列`[]`但沒大小。
{% highlight c++ linenos %}
void func1(int arr[]) {
  printf("func1 &arr= %p \n", &arr);
  printf("func1 arr= %p \n", arr);
}
{% endhighlight %}

函式參數是指標`*`或沒有大小的陣列`[]`，都是指標，目的都是接收記憶體位址。

執行以下的程式，會發現居然印出的結果跟main()函式印出的&arr、arr完全不同。
{% highlight c++ linenos %}
void func1(int arr[]) {
  printf("func1 &arr= %p \n", &arr);
  printf("func1 arr= %p \n", arr);
}
int main() {
  int arr[10];
  printf("main address= %p \n", arr);
  func1(arr);
  return 0;
}
{% endhighlight %}
```
main address= 0x7ff7bfeff2c0 
func1 &arr= 0x7ff7bfeff298 
func1 arr= 0x7ff7bfeff2c0 
```

這要回到函式的基本知識。<br>
main()複製記憶體位址給func1()函式，func1()有一個區域變數`int* arr`，arr是指標類型，內容是儲存記憶體位址。<br>

![img]({{site.imgurl}}/c++/func/arr_func_stack1.png)<br>

## 函式修改main()函式的區域變數
透過複製記憶體位址給func1，func1()可以修改main()空間中的arr區域變數。<br>
以下程式，main()函式中的arr[0]修改成11。<br>
{% highlight java linenos %}
void func1(int arr[]) {
  arr[1]++;
}
int main() {
  int arr[3] = {10, 20, 30};
  func1(arr);
  cout << "arr[0] = " << arr[0] << endl;
  cout << "arr[1] = " << arr[1] << endl;
  cout << "arr[2] = " << arr[2] << endl;
  return 0;
}
{% endhighlight %}
```
arr[0] = 10
arr[1] = 21
arr[2] = 30
```

{% highlight java linenos %}
void func1(int* arr) {
  arr[1]++;
}
int main() {
  int arr[3] = {10, 20, 30};
  func1(arr);
  cout << "arr[0] = " << arr[0] << endl;
  cout << "arr[1] = " << arr[1] << endl;
  cout << "arr[2] = " << arr[2] << endl;
  return 0;
}
{% endhighlight %}
```
arr[0] = 10
arr[1] = 21
arr[2] = 30
```