---
title: 合併排序
date: 2024-10-02
keywords: c++, Merge Sort
---

Prerequisites:

- [遞迴][1]

## 傳遞函式前處理

在遞迴有討論過二種遞迴方式，合併排序結合二種遞迴方式。

首先，先把陣列切半拆分，直到切到只剩下一個元素，就不要再拆分了。

![img]({{site.imgurl}}/dataStruct/mergeSort1.jpg)  

## 傳遞函式後處理

拆到只剩下一個元素，把一個一個的元素排序組回臨時的陣列，再copy到原本arr的陣列。


先處理已拆分成個別值6,5，二個數值比大小，比較小的放入臨時陣列(temp)，start為臨時陣列的計數器，每放進一個值，start就加1

![img]({{site.imgurl}}/dataStruct/mergeSort2.jpg)  


因已經沒得比了，把剩下的6放入臨時陣列，此時start已經是1，將排好序的臨時陣列56拷貝至原本的arr陣列
![img]({{site.imgurl}}/dataStruct/mergeSort3.jpg)  


處理已拆分成個別值3,2，二個數值比大小，比較小的放入臨時陣列(temp)，start為臨時陣列的計數器，start目前為3，每放進一個值，start就加1。
![img]({{site.imgurl}}/dataStruct/mergeSort4.jpg)  

因已經沒得比了，把剩下的3放入臨時陣列，此時start已經是4，將排好序的臨時陣列23拷貝至原本的arr陣列
![img]({{site.imgurl}}/dataStruct/mergeSort5.jpg)  


處理已拆分的二個陣列`[56]``[4]`，二個陣列分別有start1與start2，分別指著二個陣列的第1個元素。4(start2)比5(start1)小，目前start為0把4放進臨時陣列`temp[0]`start++ 與 start2++
![img]({{site.imgurl}}/dataStruct/mergeSort6.jpg)  


陣列`[56]`已經是有排序過的陣列，目前start為1，直接把陣列`[56]`所有元素放入臨時陣列
![img]({{site.imgurl}}/dataStruct/mergeSort7.jpg)  

陣列`[56]`放入臨時陣列後，把臨時陣列[456]拷貝到原本arr的陣列中
![img]({{site.imgurl}}/dataStruct/mergeSort8.jpg) 


{% highlight c++ linenos %}
void _mergeSort(int arr[], int temp[], int start, int end) {
    //列切半拆分，直到切到只剩下一個元素，start與end會是相同，就返回
    if(start >= end) return;
    //陣列切半拆分
    //若拆的陣列為[654321]，拆成[654]與[321]，要再拆分[321]，3的陣列索引是3
    // (5-3)/2 = 1 要再+3，拆分的中點索引才會在4，而不是在1
    // 拆成[34] [1]
    int mid = start + (end - start)/2;
    //拆成二半，二個陣列的起始位置
    int start1 = start, end1 = mid;
    int start2 = mid + 1, end2 = end;
    _mergeSort(arr, temp, start, mid);
    _mergeSort(arr, temp, mid+1, end);
    
    //臨時陣列計數器
    int i = start;

    //判斷二個拆分陣列的值的大小
    while(start1 <= end1 && start2 <= end2) {
        if(arr[start1] < arr[start2]) {
            temp[i++] = arr[start1++];
        } else {
            temp[i++] = arr[start2++];
        }
    }
    
    //左半邊
    while(start1 <= end1) {
        temp[i++] = arr[start1++];
    }
    //右半邊
    while(start2 <= end2) {
        temp[i++] = arr[start2++];
    }
    memcpy(arr+start, temp+start, (end - start + 1) * sizeof(int));
}
void mergeSort(int arr[],size_t len) {
    //小於2個不用比
    if(len < 2) return;
    int temp[len];
    _mergeSort(arr, temp, 0, len-1);
}
int main() {
    int arr[] = {6,5,4,3,2,1};
    int len = sizeof(arr)/sizeof(int);
    mergeSort(arr, len);
    for(int i = 0; i < len; i++) {
        cout << arr[i] << ",";
    }
    cout << endl;
    return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/dataStruct/recursion.md %}
