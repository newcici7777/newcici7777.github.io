---
title: 指標的指標
date: 2024-06-05
keywords: c++, pointer to pointer
---
指標的指標，意思是存指標的記憶體位址。<br>
下圖中，有變數i的記憶體位址0x1000，存放50。<br>
ptr本身的記憶體位址是0x1004，存放變數i的記憶體位址0x1000。<br>
pptr本身的記憶體位址是0x100c，存放的指標ptr的記憶體位址0x1004。<br>

對ptr存放的位址0x1000使用\*取值運算子，可以取得50的數值。<br>
對pptr存放的位址0x1004使用\*取值運算子，可以取得ptr的位址。<br>
對pptr存放的位址0x1004使用二次\*\*取值運算子，取得50的數值，2次取值運算子，第1次取得ptr的位址，第2次取得50。<br>
![img]({{site.imgurl}}/pointer/pptr1.png)<br>

以下有點像鏈結串列，先從pptr存的位址，使用\*取值運算子，得到ptr存的位址，使用\*取值運算子，得到變數存放的數值。<br>
![img]({{site.imgurl}}/pointer/pptr2.png)<br>

{% highlight c++ linenos %}
int main() {
  int i = 50;
  int* ptr = &i;
  int** pptr = &ptr;
  printf("&i = %p \n", &i);
  printf("&ptr = %p, ptr = %p, value = %d \n", &ptr, ptr, * ptr);
  printf("&pptr = %p, pptr = %p, value = %d \n", &pptr, pptr, ** pptr);
  return 0;
}
{% endhighlight %}
```
&i = 0x1000 
&ptr = 0x1004, ptr = 0x1000, value = 50 
&pptr = 0x100c, pptr = 0x1004, value = 50
```
## 指標的位址

{% highlight c++ linenos %}
int main() {
  int i = 40;
  cout << "i的值 = " << i << "，i的位址 = " << &i << endl;
  int *p = &i;
  cout << "指標p指向的位址 = " << p << "，指標p的位址 = " << &p << "，指標p指向位址的值=" << *p << endl;
  int **pp = &p;
  cout << "指標pp指向的位址 = " << pp << "，指標pp的位址 = " << &pp << "，指標pp指向的位址(p)指向的位址(i)=" << *pp << endl;
  cout << "指標pp指向的位址(p)指向的位址(i)的值=" << **pp << endl;
  return 0;
}
{% endhighlight %}

```
i的值 = 40，i的位址 = 0x7ff7bfeff468
指標p指向的位址 = 0x7ff7bfeff468，指標p的位址 = 0x7ff7bfeff460，指標p指向位址的值=40
指標pp指向的位址 = 0x7ff7bfeff460，指標pp的位址 = 0x7ff7bfeff458，指標pp指向的位址(p)指向的位址(i)=0x7ff7bfeff468
指標pp指向的位址(p)指向的位址(i)的值=40
```

{% highlight c++ linenos %}
int main() {
  int i4 = 40;
  //宣告一個指標存i4的位址
  int *p2 = &i4;
  //將p2指標的位址傳給pp2
  int **pp2 = &p2;

  //1.把pp2存放的位址，使用取值運算子*，也就是指標p2的位址
  //*pp2;
  //2.再把p2位址，使用取值運算子*，也就是40
  //**pp2
  printf("解出pp2的值:%d\n",**pp2);  
  return 0;
}
{% endhighlight %}

```
執行結果
解出pp2的值:40
```




