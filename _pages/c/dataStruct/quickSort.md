---
title: 快速排序
date: 2024-09-30
keywords: c++, Quick Sort
---

陣列2,5,3,6,4,1

## 值將陣列0的資料作為基準

以下圖來說，陣列0的值是2，是作為比較大小的標準。

L指向陣列0的位址

R指向陣列最後的位址

![img]({{site.imgurl}}/dataStruct/quickSort1.jpg)  

## 比較R值，尋找比基準值小

預設先比對R的值

若陣列[R]<2，將陣列[L]的值設成陣列[R]，並且L前進一格。

若以上條件不符合，R往後退一格。

```
if(陣列[R] < 2) {
	陣列[L] = 陣列[R]
	L++;
} else {
	R--;
}
```

下圖陣列[R]<2，將陣列[L]的值設成陣列[R]，並且L前進一格

![img]({{site.imgurl}}/dataStruct/quickSort2.jpg)  

## 比較L值，尋找比基準值大

若陣列[L]>2，將陣列[R]的值設成陣列[L]，並且R往後退一格。

若以上條件不符合，L往前一格。

```
if(陣列[L] > 2) {
	陣列[R] = 陣列[L]
	R--;
} else {
	L++;
}
```

下圖陣列[L]>2，將陣列[R]的值設成陣列[L]，並且R後退一格

![img]({{site.imgurl}}/dataStruct/quickSort3.jpg)  


## L==R

若陣列[R]一直找不到比2小，就會一直往後退移到跟L相同的位址。

將基準值2放入L==R的位址，第一個數字排序好

![img]({{site.imgurl}}/dataStruct/quickSort4.jpg)  

## 遞迴設定

下圖中從2(已排序好)作為中點，分為左右二半，左邊剩下1個，右邊剩下4個待排序。

左邊只剩下1個，至少要有2個數字才可以比較大小，因此視作排序完成。

把arr指標移動2格(arr+2)，從3的數字開始視作為陣列起點，大小為4個。

left的陣列位址為1，len(陣列長度)為6

```
quickSort(arr + left + 1, len - left - 1);
```
以上程式轉換如下

```
quickSort(arr + 2, 4);
```

![img]({{site.imgurl}}/dataStruct/quickSort4-1.jpg)  


## 重新設定L、R與基準值

將L指向數字3，並將比較基準值設為3，R指向陣列最尾部。

![img]({{site.imgurl}}/dataStruct/quickSort5.jpg)  

## R找不到比基準值小的值

R指標找不到比基準值3小的，最後L==R

![img]({{site.imgurl}}/dataStruct/quickSort6.jpg)  


## L==R(2)

將基準值3放入L==R的位址，第二個數字排序好

![img]({{site.imgurl}}/dataStruct/quickSort7.jpg)  


## 重新設定L、R與基準值(2)

將L指向數字6，並將基準值設為6，R指向陣列最尾部。

![img]({{site.imgurl}}/dataStruct/quickSort7.jpg)  

## 比較R值，尋找比基準值小(2)

預設先比對R的值

若陣列[R]<6，將陣列[L]的值設成陣列[R]，並且L前進一格。

![img]({{site.imgurl}}/dataStruct/quickSort8.jpg)  

## L==R(3)

將基準值6放入L==R的位址，第三個數字排序好

![img]({{site.imgurl}}/dataStruct/quickSort9.jpg)  

## 重新設定L、R與基準值(3)

![img]({{site.imgurl}}/dataStruct/quickSort10.jpg)  

## 比較R值，尋找比基準值小(3)

預設先比對R的值

若陣列[R]<5，將陣列[L]的值設成陣列[R]，並且L前進一格。

![img]({{site.imgurl}}/dataStruct/quickSort11.jpg)  

## L==R(4)

將基準值5放入L==R的位址，第四個數字排序好

![img]({{site.imgurl}}/dataStruct/quickSort12.jpg)  

## 重點步驟

- 至少要有2個數字才可以比較大小
- L指向陣列0的位址
- R指向陣列最後的位址
- 將陣列0的值作為基準值
- 預設先比較R值
- L<R,比較R值，尋找比基準值小
- L<R,比較L值，尋找比基準值大
- L==R，將基準值放入L==R的位址
- 基準值作為中點，陣列分為左右二半(不包含基準值的位址)
- 左右二半陣列繼續依上述文字進行，直至陣列個數小於2

## 完整程式碼
{% highlight c++ linenos %}
/**
 參數1 陣列位址
 參數2 陣列大小
 */
void quickSort(int arr[], int len) {
    //至少要有2個數字才可以比較大小
    if(len < 2) return;
    int left = 0;
    int right = len - 1;
    //基準值都為arr[0]
    int pivot = arr[0];
    //預設先比較R
    //action有L與R，預設先比對R的值
    char action = 'R';
    //若left<right才進入循環
    while(left < right) {
        //比較R值，尋找比基準值小
        if(action == 'R') {
            //比較R值，尋找比基準值小
            //若陣列[R]<基準值，將陣列[L]的值設成陣列[R]，並且L前進一格。
            if(arr[right] < pivot) {
                arr[left] = arr[right];
                left++;
                //設定下一次是L移動
                action = 'L';
            } else {
                //若以上條件不符合，R往後退一格。
                right--;
            }
        //比較L值，尋找比基準值大
        } else if(action == 'L') {
            //比較L值，尋找比基準值大
            //若陣列[L]>基準值，將陣列[R]的值設成陣列[L]，並且R往後退一格。
            if(arr[left] > pivot) {
                arr[right] = arr[left];
                right--;
                //設定下一次是R移動
                action = 'R';
            } else {
                //若以上條件不符合，L往前一格。
                left++;
            }
        }
    }
    //L==R
    //將基準值放入L==R的位址
    arr[right] = pivot;
    //遞迴設定
    //基準值作為中點，分為左右二半陣列(不包含基準值)
    //參數1陣列起始位址,參數2分成左右二半的各別個數
    //左半邊
    quickSort(arr,left);
    //右半邊
    //起始位址在基準值位址的下一格
    //個數(請參照遞迴設定的說明與圖)
    quickSort(arr + left + 1, len - left - 1);
}
int main() {
    int arr[] = {6,5,4,3,2,1};
    int len = sizeof(arr)/sizeof(int);
    quickSort(arr, len);
    for(int i = 0; i < len; i++) {
        cout << arr[i] << ",";
    }
    cout << endl;
    return 0;
}
{% endhighlight %}