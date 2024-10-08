---
title: ArrayList擴展容量
date: 2024-09-19
keywords: c++, ArrayList 
---

## 預設ArrayList大小，與每一次擴展容量的大小

{% highlight c++ linenos %}
//預設

#define INIT_SIZE 10
//預設擴展大小
#define EXT_SIZE 5
{% endhighlight %}

## 在結構定義最大容量

ArrayList全部分配多少記憶體大小
```
    size_t capacity;//陣列最大容量
```
{% highlight c++ linenos %}
//定義陣列中每一個元素的資料型態
typedef int ElemType;
//結構成員定義
struct ArrayList {
    ElemType *data;//陣列
    size_t size;//目前陣列中數量，由1開始
    size_t capacity;//陣列最大容量
};
{% endhighlight %}


## 清空ArrayList

結構 * 最大容量

```
    memset(&list, 0, sizeof(ElemType) * list.capacity);
```

{% highlight c++ linenos %}
/**
 清空list
 */
void ClearList(ArrayList& list) {
    list.size = 0;
    //代入的是陣列的最大容量list.capacity
    memset(&list, 0, sizeof(ElemType) * list.capacity);
}
{% endhighlight %}


## 建立ArrayList

{% highlight c++ linenos %}
void initList(ArrayList& list) {
    //初始化大小
    list.capacity = INIT_SIZE;
    //動態分配記憶體
    list.data = new ElemType[list.capacity];
    ClearList(list);
}
int main() {
    ArrayList list; //建立ArrayList
    initList(list
    return 0;
}
{% endhighlight %}

## 刪除ArrayList

{% highlight c++ linenos %}
void destroyList(ArrayList& list) {
    //釋放記憶體
    delete [] list.data;
    list.data = nullptr;
    //全部設0
    list.capacity = 0;
    list.size = 0;
}
{% endhighlight %}

## 擴展容量

步驟如下
- 建立新容量的陣列
- 將舊的陣列拷貝到新的陣列
- 刪掉舊的陣列
- 指標指向新的陣列
- 重新定義最大容量

![img]({{site.imgurl}}/dataStruct/extArrayList.jpg)  

{% highlight c++ linenos %}
bool extList(ArrayList& list) {
    int new_size = list.capacity + EXT_SIZE;
    //建立新的陣列(容量比較大的陣列)
    ElemType *new_data = new (std::nothrow) ElemType[new_size];
    //如果記憶體分配失敗
    if(new_data == nullptr) return false;
    
    //清空新的陣列
    memset(new_data, 0, sizeof(ElemType) * new_size);
    
    //拷貝舊的陣列到新的陣列
    memcpy(new_data, list.data, sizeof(ElemType) * list.size);
    
    //把舊的陣列進行記憶體釋放
    delete [] list.data;
    
    //把結構中的陣列，指向新的陣列記憶體位址
    list.data = new_data;
    
    //重新定義結構中最大容量
    list.capacity = new_size;
    return true;
}
{% endhighlight %}

## 插入

判斷是否超出最大容量
{% highlight c++ linenos %}
    //判斷是否超出最大容量
    if(list.size == list.capacity) {
        //超出容量就擴展容量
        if(!extList(list)) {
            //若返回是false，代表建立新陣列記憶體配置產生問題
            return false;
        }
    }
{% endhighlight %}


## 完整程式碼

{% highlight c++ linenos %}
//預設初始化大小
#define INIT_SIZE 10
//預設擴展大小
#define EXT_SIZE 5

//定義陣列中每一個元素的資料型態
typedef int ElemType;
//結構成員定義
struct ArrayList {
    ElemType *data;//陣列
    size_t size;//目前陣列中數量，由1開始
    size_t capacity;//陣列最大容量
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
    //代入的是陣列的最大容量list.capacity
    memset(&list, 0, sizeof(ElemType) * list.capacity);
}

void initList(ArrayList& list) {
    list.capacity = INIT_SIZE;
    list.data = new ElemType[list.capacity];
    ClearList(list);
}

void destroyList(ArrayList& list) {
    delete [] list.data;
    list.data = nullptr;
    list.capacity = 0;
    list.size = 0;
}

bool extList(ArrayList& list) {
    int new_size = list.capacity + EXT_SIZE;
    //建立新的陣列(容量比較大的陣列)
    ElemType *new_data = new (std::nothrow) ElemType[new_size];
    //如果記憶體分配失敗
    if(new_data == nullptr) return false;
    
    //清空新的陣列
    memset(new_data, 0, sizeof(ElemType) * new_size);
    
    //拷貝舊的陣列到新的陣列
    memcpy(new_data, list.data, sizeof(ElemType) * list.size);
    
    //把舊的陣列進行記憶體釋放
    delete [] list.data;
    
    //把結構中的陣列，指向新的陣列記憶體位址
    list.data = new_data;
    
    //重新定義結構中最大容量
    list.capacity = new_size;
    return true;
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
    if(list.size == list.capacity) {
        //超出容量就擴展容量
        if(!extList(list)) {
            //若返回是false，代表建立新陣列記憶體配置產生問題
            return false;
        }
    }
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
    initList(list);//初始化
    
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
    destroyList(list);
    return 0;
}
{% endhighlight %}