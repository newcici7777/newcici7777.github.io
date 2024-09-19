---
title: xml搜尋
date: 2024-09-09
keywords: c++, xml search 
---

Prerequisites:

- [記憶體間隔計算][1]



要在一堆xml內容中尋找標籤名為name，並取出值。

```
<id>1</id><name>Cici</name><age>10</age>
```

## 思路

### 組成name開始標籤與結束標籤

自已建立`<name>`字串，並在xml中尋找是否有相同的字串，函式會回傳`<name>`起始位址。

|xml|`<`|n|a|m|e|`>`|
|回傳位址|^||||||


自已建立`</name>`字串，並在xml中尋找是否有相同的字串，函式會回傳`</name>`起始位址。

|xml|`<`|n|a|m|e|`>`|C|i|c|i|`<`|`/`|n|a|m|e|`>`|
|回傳位址|||||||||||^|||||||


### 計算值的字元個數

假設位址以0x00000001開始

|位址|01|02|03|04|05|06|07|08|09|10|11|12|13|14|15|16|17
|xml|`<`|n|a|m|e|`>`|C|i|c|i|`<`|`/`|n|a|m|e|`>`|
|findEnd|||||||||||^|||||||
|findStart|^|||||||||||||||||

值的字元個數計算方式，取得findEnd記憶體位址 - findStart記憶體位址 - 左右括號 - 標籤名 

```
findEnd(11) - findStart(1) - `<>`左右括號(2) - xml標籤名name(4) = 4 
```

{% highlight c++ linenos %}
size_t valueLen = findEnd - findStart - fieldLen - 2;
{% endhighlight %}


### 截取值

指標移動2格`<>`左右括號，再移動4格`name`標籤名的長度，就會移動到值的起始位址。

上一個步驟有算出，值的長度為4個字元，從移動後的指標，往後取得4個字元(包含移動後指標當前的值)。

|位址|01|02|03|04|05|06|07|08|09|10|11|12|13|14|15|16|17
|xml|`<`|n|a|m|e|`>`|C|i|c|i|`<`|`/`|n|a|m|e|`>`|
|移動前|^|||||||||||||||||
|移動後|||||||^|||||||||||
|取值|||||||x|x|x|x||||||||

{% highlight c++ linenos %}
strncpy(values, findStart + 2 + fieldLen , valueLen);
{% endhighlight %}

## 完整程式碼

{% highlight c++ linenos %}
/**
 xmlbuffer xml字串
 fieldname 要搜尋的 標籤名
 values存放找到的值
 **/
bool xmlSearch(const char* xmlbuffer, const char* fieldname, char* values) {
    //檢查參數有沒有值
    if(xmlbuffer == 0 || fieldname == 0 || values == 0) return false;
    
    //搜尋的欄位名大小
    size_t fieldLen = strlen(fieldname);
    //組成xml搜尋字串
    //例，找xml標籤名是name的值
    //開始 標籤名的字串
    //<name>\0
    //字串大小為2個<>尖括號+1個結尾空字元0+標籤名name大小
    char* startField = new char[2 + 1 + fieldLen];
    //清空陣列
    memset(startField, 0, sizeof(startField));
    strcpy(startField, "<"); strcat(startField, fieldname); strcat(startField, ">");
    //結束 標籤名的字串
    //</name>\0
    //字串大小為3個</>符號+1個結尾空字元0+標籤名name大小
    char* endField = new char[3 + 1 + fieldLen];
    //清空陣列
    memset(endField, 0, sizeof(endField));
    strcpy(endField, "</"); strcat(endField, fieldname); strcat(endField, ">");
    //搜<name>
    char* findStart = (char*)strstr(xmlbuffer, startField);
    //搜</name>
    char* findEnd = (char*)strstr(xmlbuffer, endField);
    //在xml字串中找到<name>與</name>
    if(findStart && findEnd) {
        //截取<name>.......</name>中間的值，儲存在value字串
        //計算<name>.......</name>中間的值個數
        size_t valueLen = findEnd - findStart - fieldLen - 2;
        //findStart + 2 + fieldLen 是指將指標移動到<name>的後面
        //以下第三個參數代表拷貝個數
        strncpy(values, findStart + 2 + fieldLen , valueLen);
        
        //清除記憶空間
        delete[] startField; startField = nullptr;
        delete[] endField; endField = nullptr;
        return true;
    }
    //找不到
    //清除記憶空間
    delete[] startField; startField = nullptr;
    delete[] endField; endField = nullptr;
    return false;
}
int main() {
    char values[100];
    //清空陣列
    memset(values, 0, sizeof(values));
    
    bool res = xmlSearch("<id>1</id><name>Cici</name><age>10</age>", "name", values);
    //如果有找到會回傳true
    if(res) {
        cout << "value = " << values << endl;
    }
    return 0;
}
{% endhighlight %}

```
value = Cici
```

[1]: {% link _pages/c/dynamicMemory/memory_interval.md %}