---
title: 原地反轉字串
date: 2024-07-27
keywords: c++, reverse a string in-place 
---

原地的意思是只在原本的陣列移動並反轉字串。
不額外建立其它記憶體空間。

## 思路如下

### 偶數字串

字串abcd從中間切一半，並頭尾向中間逐一進行交換。

ab cd

a與d交換

b與c交換

### 奇數字串

字串abcde，中間值為c，除了c不用變，其它字母，頭尾向中間逐一進行交換。

ab c de

a與e交換

b與d交換

## 程式碼

以abcde為例，長度為5，迴圈次數 5 / 2 = 2次

以下為互相交換的陣列索引

|迴圈次數|i|len-1-i|
|:--:|:--:|:--:|
|第1次|0|4|
|第2次|1|3|


{% highlight c++ linenos %}
int main() {
  char str[100];
  //清空記憶體的值
  memset(str,0,sizeof(str));
  cout << "請輸入字串:"; cin >> str;
  
  for(int i = 0, len = strlen(str); i < len/2; i++) {
    char temp = str[i];
    str[i] = str[len - 1 - i];
    str[len - 1 - i] = temp;
  }
  cout << str << endl;
  return 0;
}
{% endhighlight %}

