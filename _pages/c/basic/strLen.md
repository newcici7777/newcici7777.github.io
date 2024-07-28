---
title: 取得字串長度
date: 2024-07-26
keywords: c++, loop
---
Prerequisites:

- [空字元][1]
- [字串清空] [2]
- [迴圈][3]

實作strlen()函式

## 取得字串長度
### 陣列法
{% highlight c++ linenos %}
int length(char* str) {
    //初始值
    int len = 0;
    //條件判斷式為字元不等於\0，進入迴圈
    for(;str[len] != 0; len++);
    return len;
}
int main() {
    char str[100];
    //清空記憶體的值
    memset(str,0,sizeof(str));
    cout << "請輸入字串:"; cin >> str;
    cout << "字串長度:" << length(str) << endl;
    return 0;
}
{% endhighlight %}
```
請輸入字串:abcde
字串長度:5
```
### 指標法1
{% highlight c++ linenos %}
int length(char* str) {
    int len = 0;
    //指標移到字串陣列[0]的位址
    char *ptr = str;
    //判斷字元
    while(*ptr != 0) {
    	//位址往下一個陣列索引移動
        ptr++;
        //記錄字串長度
        len++;
    }
    return len;
}
{% endhighlight %}

精簡版
{% highlight c++ linenos %}
int length(char* str) {
    int len = 0;
    char *ptr = str;
    while(*ptr++) len++;
    return len;
}
{% endhighlight %}

### 指標法2

{% highlight c++ linenos %}
int length(char* str) {
    int len = 0;
    for(char *pos = str; *pos; pos++, len++);
    return len;
}
{% endhighlight %}

### 遞迴法
{% highlight c++ linenos %}
int length(char* str) {
    //離開遞迴基本條件base case
    if(*str == 0) return 0;
    //若字元不為空字元，就執行以下程式碼
    //指標往下個位址移動
    str++;
    //類似迴圈len++
    return length(str) + 1;
}
{% endhighlight %}


## 印出字串

### 100次迴圈

{% highlight c++ linenos %}
int main() {
    char str[100];
    //清空記憶體的值
    memset(str,0,sizeof(str));
    cout << "請輸入字串:"; cin >> str;
    for(char *pos = str; *pos; pos++)
        cout << *pos;
    cout << endl;
    return 0;
}
{% endhighlight %}

```
請輸入字串:abcdef
abcdef
```

### 1000次迴圈

for執行100次

strlen()執行100次

100*100 = 1000次迴圈

{% highlight c++ linenos %}
int main() {
    char str[100];
    //清空記憶體的值
    memset(str,0,sizeof(str));
    cout << "請輸入字串:"; cin >> str;
    
    for(int i = 0; i < strlen(str); i++) {
        cout << str[i];
    }
    cout << endl;
    return 0;
}
{% endhighlight %}

### 200次迴圈

將以上程式碼改善，把strlen移到初始運算式或定義在for迴圈之前，初始運算式只執行一次。

{% highlight c++ linenos %}
int main() {
    char str[100];
    //清空記憶體的值
    memset(str,0,sizeof(str));
    cout << "請輸入字串:"; cin >> str;
    
    for(int i = 0, len = strlen(str); i < len; i++) {
        cout << str[i];
    }
    cout << endl;
    return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/array/charArray.md %}#空字元null-character
[2]: {% link _pages/c/array/charArray.md %}#字串清空
[3]: {% link _pages/c/basic/loop.md %}