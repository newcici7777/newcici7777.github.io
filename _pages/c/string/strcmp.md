---
title: 字串比較
date: 2024-08-02
keywords: c++, strcmp 
---

## strcmp

{% highlight c++ linenos %}
/**
 字串相等傳回0
 str1字元>str2字元 return 1
 str1字元<str2字元 return -1
 **/
int mystrcmp(const char* str1,const char* str2) {
  //特別注意，使用while(true)，即便str1或str2是\0也能比較
  while(true){
    //str1字元>str2字元
    if(*str1 > *str2) return 1;
    //str1字元<str2字元
    if(*str1 < *str2) return -1;
    //二個字元結尾相等
    if(*str1 == 0 && *str2 == 0) return 0;
    //比較下一個字元
    str1++;
    str2++;
  }
}
int main() {
  cout << mystrcmp("abc","abcd") << endl;
  cout << mystrcmp("ab","a") << endl;
  cout << mystrcmp("abc","abc") << endl;
  cout << mystrcmp("","a") << endl;
  return 0;
}
{% endhighlight %}

```
-1
1
0
-1
```

## strncmp

```
int mystrncmp(const char* str1,const char* str2,const size_t n)
```

第一個參數是比較字串1。

第二個參數是比較字串2。

第三個參數n是要比較多少字元，若n為2，比較字串1與字串2前2個字元。

{% highlight c++ linenos %}
int mystrncmp(const char* str1,const char* str2,const size_t n) {
  int i = 0;
  while(i < n){
    //str1字元>str2字元
    if(*str1 > *str2) return 1;
    //str1字元<str2字元
    if(*str1 < *str2) return -1;
    //二個字元結尾相等
    if(*str1 == 0 && *str2 == 0) return 0;
    //比較下一個字元
    str1++;
    str2++;
    i++;
  }
  //比較n次仍比較不出來，就傳回0
  return 0;
}
int main() {
  cout << mystrncmp("abc","abcd",2) << endl;
  cout << mystrncmp("ab","a",2) << endl;
  cout << mystrncmp("abc","abc",2) << endl;
  return 0;
}
{% endhighlight %}

```
0
1
0
```