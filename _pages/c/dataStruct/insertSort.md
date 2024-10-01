---
title: 插入排序
date: 2024-09-28
keywords: c++, Insertion sort
---

## 插入過程圖

假設目前位置在5的數字，往前尋找比5還小的數字，把5放在比5還小的數字的後面，若尋找的過程中，數字比5還大，就把數字往後移一位。

i是目前位置

j就是往前尋找比5還小的數字的計數變數

### 數字比5還大，就把數字往後移一位

下圖arr[i] = 5，arr[j] = 44，44 > 5，把44往後移一位，arr[j + 1] = 44

![img]({{site.imgurl}}/dataStruct/insertSort1.jpg)  

下圖arr[i] = 5，arr[j] = 38，38 > 5，把38往後移一位，arr[j + 1] = 38

![img]({{site.imgurl}}/dataStruct/insertSort2.jpg)  

### 尋找比5還小的數字，把5放在比5還小的數字的後面

下圖arr[i] = 5，arr[j] = 3，3 < 5，把5放在3的後面

![img]({{site.imgurl}}/dataStruct/insertSort3.jpg) 

## 變數起始位置

i起始位置為1，因為arr[0]前面沒有值可以比較，所以把i的一開始的位置定義在arr[1]

j的起始位置為i-1，也就是從i的位置以前(不含i)去搜尋有沒有比arr[i]還小的數字，若比arr[i]大，就把arr[j]的值往後移動一格arr[j+1]

![img]({{site.imgurl}}/dataStruct/insertSort4.jpg) 

## 二個迴圈

外圍的迴圈，是遍歷每一個數字

內圍的迴圈，是尋找外圍數字之前有沒有比它小的數字。

## 完整程式碼
{% highlight c++ linenos %}
void insertSort(int arr[], int len) {
    for(int i = 1; i < len; i++) {
        int temp = arr[i];
        int j = i - 1;
        for(; j >= 0; j--) {
            if(arr[j] > temp) {
                arr[j + 1] = arr[j];
            } else {
                //若i=1,j=0
                //6放進arr[0+1]之後，就會離開這個迴圈，因為j--，j=-1
                //j=-1，就不會執行以下這一行
                break;
            }
        }
        //若i=1,j=0
        //6放進arr[j+1]之後，就會離開迴圈，j--，j=-1
        //arr[1]=6
        //arr[-1 + 1] = 5
        //arr[0] = 5
        arr[j + 1] = temp;
    }
}
int main() {
    int arr[] = {6,5,4,3,2,1};
    int len = sizeof(arr)/sizeof(int);
    insertSort(arr, len);
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