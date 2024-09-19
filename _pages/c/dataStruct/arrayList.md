---
title: ArrayList底層實作
date: 2024-09-16
keywords: c++, ArrayList 
---

Prerequisites:

- [記憶體間隔計算][1]


## 名詞解釋

list.size 陣列有幾個元素？
以下舉例的例子都是10個元素。


## 結構定義

{% highlight c++ linenos %}
//陣列最大的大小
#define MAXSIZE 100
//定義陣列中每一個元素的資料型態
typedef int ElemType;
//結構成員定義
struct ArrayList {
    ElemType data[MAXSIZE];//陣列
    size_t size;//目前陣列中數量，由1開始
};
{% endhighlight %}

## 清空所有元素

使用前確保所有元素都清空。

{% highlight c++ linenos %}
/**
 清空list
 */
void ClearList(ArrayList& list) {
    list.size = 0;
    memset(&list, 0, sizeof(ElemType) * MAXSIZE);
}
{% endhighlight %}

## 插入

位址轉成10進位，比較好理解

|位址|1|2|3|4|5|6|7|8|9|10|
|索引|0|1|2|3|4|5|6|7|8|9|
|值|11|12|13|14|15|16|17|18|19|20|

假設要在索引4，插入一筆資料，必須將含索引4以後的資料往後移動1格。

list[index ...] -> list[index + 1  ...]

移動資料前

|索引|0|1|2|3|4|5|6|7|8|9|
|值|11|12|13|14|15|16|17|18|19|20|

移動資料後

|索引|0|1|2|3|4|5|6|7|8|9|10|
|值|11|12|13|14|x|15|16|17|18|19|20|

把x更換成要插入的值。


### 目的起始位址與來源起始位址

假設要在索引4，插入一筆資料，必須將含索引4以後的資料往後移動1格。

![img]({{site.imgurl}}/dataStruct/srcAddress.jpg)  

![img]({{site.imgurl}}/dataStruct/descAddress.jpg)  

list[index ...] 拷貝-> list[index + 1  ...]

來源起始位址 拷貝-> 目的起始位址

```
memmove(目的起始位址, 來源起始位址, 拷貝個數)
```

|位址|1|2|3|4|5|6|7|8|9|10|
|索引|0|1|2|3|4|5|6|7|8|9|
|值|11|12|13|14|15|16|17|18|19|20|
|data指標|^||||||||||
|來源起始位址|||||^||||||
|目的起始位址||||||^|||||


將(data指標 + 4)位址之後的資料，移到後一格(data指標 + (4 + 1))。

來源起始位址
```
list.data + index
```

目的起始位址
```
list.data + (index + 1)
```

### 拷貝個數

拷貝[含]索引4以後的個數

|索引|0|1|2|3|4|5|6|7|8|9|
|值|11|12|13|14|15|16|17|18|19|20|
|拷貝個數|||||^|^|^|^|^|^|

求拷貝個數

![img]({{site.imgurl}}/dataStruct/restCount.jpg)  

全部數量為10，扣掉不用拷貝的4個數量，剩下的就是我們要拷貝的。

list.size = 10，代表全部有10個元素。

10(全部數量) - 4(不用拷貝的數量) = 6(要拷貝的數量)

```
list.size - index
```

{% highlight c++ linenos %}
        memmove(list.data + (index + 1), list.data + index , (list.size - index) * sizeof(ElemType));
{% endhighlight %}


### 最後一個位址可以插入

![img]({{site.imgurl}}/dataStruct/insertLast.jpg)  

參數index索引等於list.size，直接新增資料，不需搬移資料

{% highlight c++ linenos %}
memcpy(&list.data[list.size], &element, sizeof(ElemType));
{% endhighlight %}

### 插入程式碼

{% highlight c++ linenos %}
/**
 插入元素
 成功返回true，失敗返回false
 index為陣列索引，介於0 - list.size之間，index為list.size也可以插入
 element為插入元素
 
 搬移
 list[index...] -> list[index + 1 ...]
 */
bool insert(ArrayList& list, const size_t index, const ElemType& element) {
    //判斷是否超出最大容量
    if(list.size == MAXSIZE) return false;
    //檢查插入索引
    if(index > list.size) {
        cout << "insert的索引介於0-" << list.size  << endl;
        return false;
    }
    //insert的索引在中間，並非最後一個
    //若插入的索引 == size，不用搬移資料，直接在最後面新增資料。
    if(index < list.size) {
        //搬移資料
        //list[index...] -> list[index + 1 ...]
        memmove(list.data + (index + 1), list.data + index , (list.size - index) * sizeof(ElemType));
    }
    //插入資料
    memcpy(&list.data[index], &element, sizeof(ElemType));
    //list的增加一個
    list.size++;
    return true;
}
{% endhighlight %}

## 刪除


### 往前拷貝

刪除索引4，索引4以後的元素(5 ... size-1)往前移一位

![img]({{site.imgurl}}/dataStruct/del.jpg)  

 list[index + 1 ...] 拷貝-> list[index  ...]


### 要拷貝的數量

![img]({{site.imgurl}}/dataStruct/del2.jpg)

全部數量為10，扣掉不用拷貝的4個數量，剩下的就是我們要拷貝的。

list.size = 10，代表全部有10個元素。

剩下的就是我們要拷貝的，要減掉一個刪除元素。

10(全部數量) - 4(不用拷貝的數量) - 1(拷貝的數量中有1個要刪除) = 5(要拷貝的數量)

```
list.size - index - 1
```

{% highlight c++ linenos %}
    //list[index + 1 ...] 拷貝-> list[index ...]
    //因為要刪除一個元素，所以拷貝的個數要-1
    memmove(list.data + index, list.data + (index + 1), (list.size - index - 1) * sizeof(ElemType));
{% endhighlight %}

### 最後一個位址不用搬移資料

參數index索引等於list.size - 1，直接刪除資料，不需搬移資料

### 刪除程式碼

{% highlight c++ linenos %}
/**
 刪除元素
 成功返回true，失敗返回false
 index為陣列索引，介於0 - (list.size-1)
 list[index+1...] 拷貝至->  list[index  ...]
 */
bool del(ArrayList& list, const size_t index) {
    //大小為0，不用刪除任何資料
    if(list.size == 0) return false;
    //檢查刪除索引
    if(!checkIndex(list, index)) {
        cout << "delete索引介於0-" << list.size - 1  << endl;
        return false;
    }
    //若index不為最後一筆，要進行搬移
    if(index < (list.size - 1)) {
        //list[index + 1 ...] 拷貝-> list[index ...]
        memmove(list.data + index, list.data + (index + 1), (list.size - index - 1) * sizeof(ElemType));
    }
    
    //總數量減1
    list.size--;
    return true;
}

{% endhighlight %}

## 印出所有元素

{% highlight c++ linenos %}
void printList(const ArrayList& list) {
    for(size_t i = 0; i < list.size; i++) {
        cout << list.data[i] << endl;
    }
}
{% endhighlight %}

## 測試程式碼

{% highlight c++ linenos %}
int main() {
    ArrayList list; //建立ArrayList
    ClearList(list);//清空
    
    //ElemType是int
    ElemType element;
    
    element = 11;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 12;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 13;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 14;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 15;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 16;
    //在索引0插入元素
    insert(list, list.size, element);
    printList(list);
    cout << "---------------------" << endl;
    del(list,0);
    printList(list);
    return 0;
}
{% endhighlight %}

```
15
14
13
12
11
16
---------------------
14
13
12
11
16
```

## 完整程式碼
{% highlight c++ linenos %}
//陣列最大的大小
#define MAXSIZE 100
//定義陣列中每一個元素的資料型態
typedef int ElemType;
//結構成員定義
struct ArrayList {
    ElemType data[MAXSIZE];//陣列
    size_t size;//目前陣列中數量，由1開始
};

/**
 索引是否介於0... size-1
 */
bool checkIndex(const ArrayList& list, const size_t index) {
    return index < list.size;
}
/**
 清空list
 */
void ClearList(ArrayList& list) {
    list.size = 0;
    memset(&list, 0, sizeof(ElemType) * MAXSIZE);
}
/**
 插入元素
 成功返回true，失敗返回false
 index為陣列索引，介於0 - list.size之間，index為list.size也可以插入
 element為插入元素
 
 搬移
 list[index...] -> list[index + 1 ...]
 */
bool insert(ArrayList& list, const size_t index, const ElemType& element) {
    //判斷是否超出最大容量
    if(list.size == MAXSIZE) return false;
    //檢查插入索引
    if(index > list.size) {
        cout << "insert的索引介於0-" << list.size  << endl;
        return false;
    }
    //insert的索引在中間，並非最後一個
    //若插入的索引 == size，不用搬移資料，直接在最後面新增資料。
    if(index < list.size) {
        //搬移資料
        //list[index...] -> list[index + 1 ...]
        memmove(list.data + (index + 1), list.data + index , (list.size - index) * sizeof(ElemType));
    }
    //插入資料
    memcpy(&list.data[index], &element, sizeof(ElemType));
    //list的增加一個
    list.size++;
    return true;
}
/**
 刪除元素
 成功返回true，失敗返回false
 index為陣列索引，介於0 - (list.size-1)
 list[index+1...] 拷貝至->  list[index  ...]
 */
bool del(ArrayList& list, const size_t index) {
    //大小為0，不用刪除任何資料
    if(list.size == 0) return false;
    //檢查刪除索引
    if(!checkIndex(list, index)) {
        cout << "delete索引介於0-" << list.size - 1  << endl;
        return false;
    }
    //若index不為最後一筆，要進行搬移
    if(index < (list.size - 1)) {
        //list[index + 1 ...] 拷貝-> list[index ...]
        memmove(list.data + index, list.data + (index + 1), (list.size - index - 1) * sizeof(ElemType));
    }
    
    //總數量減1
    list.size--;
    return true;
}

void printList(const ArrayList& list) {
    for(size_t i = 0; i < list.size; i++) {
        cout << list.data[i] << endl;
    }
}

/**
 返回索引
 找不到就返回-1
 */
int findElem(const ArrayList& list, const ElemType& e) {
    for(int i = 0; i < list.size; i++) {
        if(list.data[i] == e) return i;
    }
    return -1;
}

/**
 取得元素
 參數2index:索引
 參數3:e 返回元素
 */
bool getElem(const ArrayList& list, const size_t index,ElemType& e) {
    if(!checkIndex(list, index)) return false;
    e = list.data[index];
    return true;
}

int main() {
    ArrayList list; //建立ArrayList
    ClearList(list);//清空
    
    //ElemType是int
    ElemType element;
    
    element = 11;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 12;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 13;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 14;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 15;
    //在索引0插入元素
    insert(list, 0, element);
    
    element = 16;
    //在索引0插入元素
    insert(list, list.size, element);
    printList(list);
    cout << "---------------------" << endl;
    del(list,0);
    printList(list);
    return 0;
}
{% endhighlight %}


[1]: {% link _pages/c/dynamicMemory/memory_interval.md %}