---
title: 氣泡排序
date: 2024-09-26
keywords: c++, bubbleSort 
---

## 交換過程圖

![img]({{site.imgurl}}/dataStruct/bubbleSort1.jpg)  

![img]({{site.imgurl}}/dataStruct/bubbleSort2.jpg)  

![img]({{site.imgurl}}/dataStruct/bubbleSort3.jpg)  

![img]({{site.imgurl}}/dataStruct/bubbleSort4.jpg)  

![img]({{site.imgurl}}/dataStruct/bubbleSort5.jpg)  


## 比較大小

陣列大小為6，以下為陣列的索引

|0|1|2|3|4|5|

### i與j變數解釋

i變數為最大輪數與最大次數。

以本例來說，最大輪數為5輪

以本例來說，比較最大次數5次

j變數為比較次數的計數器，arr[j]與arr[j+1]，二個相鄰的變數比較大小

例

j = 0，arr[0]與arr[1]比較大小

j = 1，arr[1]與arr[2]比較大小

j = 2，arr[2]與arr[3]比較大小

j = 3，arr[3]與arr[4]比較大小

j = 4，arr[4]與arr[5]比較大小


### 第1輪 i =5

i最大比較次數 i = 5

j < i最大次數5

j的索引0...4

arr[j]與arr[j+1]，二個互相比較

|索引|0|1|2|3|4|5|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|j=0|0|1|||||
|j=1||1|2||||
|j=2|||2|3|||
|j=3||||3|4||
|j=4|||||4|5|

### 第2輪 i =4

i最大比較次數 i = 4

j < i最大次數4

j的索引0...3

arr[j]與arr[j+1]，二個互相比較

|索引|0|1|2|3|4|5|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|j=0|0|1|||||
|j=1||1|2||||
|j=2|||2|3|||
|j=3||||3|4||

### 第3輪 i = 3

i最大比較次數 i = 3

j < i最大次數3

j的索引0...2

arr[j]與arr[j+1]，二個互相比較

|索引|0|1|2|3|4|5|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|j=0|0|1|||||
|j=1||1|2||||
|j=2|||2|3|||

### 第4輪 i = 2

i最大比較次數 i = 2

j < i最大次數2

j的索引0...1

arr[j]與arr[j+1]，二個互相比較

|索引|0|1|2|3|4|5|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|j=0|0|1|||||
|j=1||1|2||||

### 第5輪 i = 1

i最大比較次數 i = 1

j < i最大次數1

j的索引0

arr[0]與arr[0+1]，二個互相比較

|索引|0|1|2|3|4|5|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|j=0|0|1|||||





## 完整程式碼

{% highlight c++ linenos %}
using namespace std;
void bubbleSort(int arr[], int len) {
    for(int i = len-1; i > 0; i--) {
        for(int j = 0; j < i; j++) {
            if(arr[j] > arr[j+1]) {
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}
int main() {
    int arr[] = {6,5,4,3,2,1};
    int len = sizeof(arr)/sizeof(int);
    bubbleSort(arr, len);
    
    for(int i = 0; i < len; i++) {
        cout << arr[i] << ",";
    }
    cout << endl;
    return 0;
}
{% endhighlight %}

```
1,2,3,4,5,6,
```