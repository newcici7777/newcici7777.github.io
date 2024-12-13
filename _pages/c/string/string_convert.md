---
title: string convert
date: 2024-12-11
keywords: c++, string
---

Prerequisites:

- [string][1]


## to_string

將整數轉成string
{% highlight c++ linenos %}
  string str = to_string(2022 + 23);
  cout << "str = " << str << endl;
{% endhighlight %}
```
str = 2045
```

## string to char指標

{% highlight c++ linenos %}
  // char指標陣列
  char* arr[10];
  arr[0] = (char*)"test";
  arr[1] = (char*)"/123";
{% endhighlight %}

## const string to char指標

- [strcpy][2]
- [c_str][3]

c_str()是取得string物件位址

{% highlight c++ linenos %}
  const string title = "被討厭的勇氣";
  char* s = new char[100];
  strcpy(s, title.c_str());
  cout << "s = " << s << endl;
{% endhighlight %}
```
s = 被討厭的勇氣
```

[1]: {% link _pages/c/stl/string.md %}
[2]: {% link _pages/c/array/charArray.md %}#字串指標拷貝-strcpy
[3]: {% link _pages/c/stl/string.md %}#取得string物件存放字串的記憶體位址