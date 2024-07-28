---
title: 整數轉成字串
date: 2024-07-29
keywords: c++ , integer to string
---

Prerequisites:

- [原地反轉字串][1]
- [字元運算][2]

## 思路

輸入1234

轉成\'1234\'

定義字元陣列 char result[20];

定義索引 int index = 0;

### 步驟1

1234 % 10 = 4

1234 / 10 = 123

將餘數4轉成字元存入陣列

```
result[0] = 4 + '0';
```

### 步驟2

123 % 10 = 3

123 / 10 = 12

將餘數3轉成字元存入陣列

```
result[1] = 3 + '0';
```

### 步驟3

12 % 10 = 2

12 / 10 = 1

將餘數2轉成字元存入陣列

```
result[2] = 2 + '0';
```

### 步驟4

1 % 10 = 1

1 / 10 = 0

將餘數1轉成字元存入陣列

```
result[3] = 1 + '0';
```
### 翻轉字串

將\'4321\'轉成\'1234\'


## 完整程式碼

{% highlight c++ linenos %}
int main() {
    int input = 0;
    cout << "請輸入整數:"; cin >> input;
    char result[20];
    memset(result, 0, sizeof(result));
    //取餘數
    int index = 0;
    while(input > 0) {
    	//將餘數轉成字元存入陣列
        result[index++] = input % 10 + '0';
        input = input / 10;
    }
    cout << "result : " << result << endl;
    //翻轉
    for(int i = 0, len = strlen(result); i < len/2; i++) {
        char temp = result[i];
        result[i] = result[len - 1 - i];
        result[len - 1 - i] = temp;
    }
    cout << "Reverse result : " << result << endl;
    return 0;
}
{% endhighlight %}


```
請輸入整數:1234
result : 4321
Reverse result : 1234
```


[1]: {% link _pages/c/array/reverseStr.md %}
[2]: {% link _pages/c/basic/charType.md %}#字元運算