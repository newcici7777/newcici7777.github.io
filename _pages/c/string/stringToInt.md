---
title: 字串轉成數字
date: 2024-07-29
keywords: c++ ,  string to integer
---

## 思路

將\`1234\`轉成1234

定義int result = 0，作為乘數。

用迴圈遍歷字串

### 步驟1

input[0]是\`1\`，把\`1\`變成整數1。

\'1\' - \'0\' = 1

result = 0

(result * 10) + 1 = 1

result = 1

### 步驟2

input[1]是\`2\`，把\`2\`變成整數2。

\'2\' - \'0\' = 2

result = 1

(result * 10) + 2 = 12

result = 12

### 步驟3

input[2]是\`3\`，把\`3\`變成整數3。

\'3\' - \'0\' = 3

result = 12

(result * 10) + 3 = 123

result = 123

### 步驟4

input[3]是\`4\`，把\`4\`變成整數4。

\'4\' - \'0\' = 4

result = 123

(result * 10) + 4 = 1234

result = 1234


## 完整程式碼

{% highlight c++ linenos %}
int main() {
    char input[10];
    memset(input, 0, sizeof(input));
    cout << "請輸入整數:"; cin >> input;
    int result = 0;
    
    for(int i = 0, len = strlen(input); i < len; i++) {
        result = (result * 10) + input[i] - '0';
    }
    cout << "result = " << result << endl;
    return 0;
}
{% endhighlight %}

```
請輸入整數:1234
result = 1234
```

