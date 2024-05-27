---
title: 記憶體配置
date: 2024-05-06
keywords: c++, 記憶體佈局
---


### 記憶體起始與結束地址。
記憶體開始地址由下表最下方開始，記憶體結束地址在最上方。


<table>
  <tr>
    <th align="center" width="10%">開始與結束</th>
    <th align="left" width="10%">地址高低</th>
    <th align="left">地址</th>
  </tr>  
  <tr>
      <td>結束</td>
      <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
        <span style="font-size: 20pt">高</span>
      </td>
      <td>0xFFFFFFFF</td>
  </tr>
  <tr><td></td><td>0xFFFFFFFE</td></tr>
  <tr><td></td><td>0xFFFFFFFD</td></tr>
  <tr><td></td><td>0xFFFFFFFC</td></tr>
  <tr>
    <td></td>
    <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
        <span style="font-size: 30pt">↓</span>
    </td>
    <td>......</td></tr>
  <tr><td></td><td>......</td></tr>
  <tr><td></td><td>......</td></tr>
  <tr><td></td><td>......</td></tr>
  <tr>
    <td></td>
    <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
        <span style="font-size: 20pt">低</span>
    </td>
    <td>0x00000004</td></tr>
  <tr><td></td><td>0x00000002</td></tr>
  <tr><td></td><td>0x00000001</td></tr>
  <tr><td>開始</td><td>0x00000000</td></tr>
</table>

### 記憶體區段

記憶體區段根據地址由高到低分別為Kernel, Stack, 尚位使用區域, Heap, bass, data, code。

<table>
  <tr>
    <th align="center" width="5%">地址高低</th>
    <th align="left" width="30%">區段</th>
    <th align="left" width="5%">地址增長方向</th>
    <th align="left">儲存項目</th>
  </tr>
  <tr>
    <td>高</td>
    <td>Kernel</td>
    <td></td>
    <td>內核</td>
  </tr>
  <tr>
    <td rowspan="5" style="vertical-align: middle;">
        <span style="font-size: 10pt">↓</span>
    </td>
    <td>Stack</td>
    <td>&#8595;</td>
    <td>區域變數</td>
  </tr>
  <tr>
    <td>尚未使用區域</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Heap</td>
    <td>&#8593;</td>
    <td>動態分配指標</td>
  </tr>
  <tr>
    <td>bss segmen</td>
    <td></td>
    <td>未初始化全域變數, 靜態變數</td>
  </tr>
  <tr>
    <td>data segmen</td>
    <td></td>
    <td>已初始化全域變數, 靜態變數</td>
  </tr>
  <tr>
    <td>低</td>    
    <td>code segment</td>
    <td></td>
    <td>常數與程式執行檔</td>
  </tr>                
</table>

#### 內核

處理cpu記憶體Devices與應用程式運作。

#### stack

函式或程式區塊`{}`中區域變數與函式參數與函式返回值的記憶體地址。

#### Heap

動態分配指標記憶體地址

#### bss

未初始化全域變數與靜態變數記憶體地址

#### data segmen

已初始化全域變數與靜態變數記憶體地址

#### code segment

常數與程式執行檔地址

### 變數記憶體位址

{% highlight c++ linenos %}
#include <stdio.h>
const int global_x = 1;  // 儲存於 code segment(常數)
int global_y = 1;        // 儲存於 data segmen(已初始化全域變數）
int global_z;            // 儲存於 bss(未初始全域變數)
int fun1(int param1) {	 // 儲存於 stack (函式參數)
	return param1; // 儲存於 stack (函式返回值)
}
int main() {
  const static int x = 1; // 儲存於 code segment(常數)
  static int y = 1;       // 儲存於 data segmen(已初始化靜態變數）
  static int z;           // 儲存於 bss(未初始靜態變數)
  int w = 1;              // 儲存於 stack (區域變數)
  fun1(w);

  // 儲存於 heap (動態分配指標)
  char *buf = (char*) malloc(sizeof(char) * 100);
  // ...
  free(buf);

  int* p = new int(3); // 儲存於 heap (動態分配指標)
  delete p;
  return 0;
}
{% endhighlight %}

### Stack
* 儲存在Stack的變數，變數離開有效範圍(Scope)後，會由系統自動釋放記憶體地址。
* Stack記憶體容量8M。
* 記憶體地址向下遞減。
* 不會memory leak。

以下程式碼在函式中建立三個變數，並觀察三個變數的記憶體地址是由大至小遞減。證明記憶體地址向下遞減。

{% highlight c++ linenos %}
void funcMemoryLocation() {
    int var1 = 10;
    int var2 = 20;
    int var3 = 30;
    cout << "va1 = " << (long long)&var1 << endl;
    cout << "va2 = " << (long long)&var2 << endl;
    cout << "va3 = " << (long long)&var3 << endl;
}
{% endhighlight %}
```
執行結果
va1 = 140702053822444
va2 = 140702053822440
va3 = 140702053822436
```

### Heap
* 儲存在Heap的指標，由程式設計師手動釋放記憶體地址，或待主程式生命周期結束後被系統釋放記憶體地址。
* Heap記憶體容量取決於電腦的記憶體大小。
* 記憶體地址向上遞增。
* 會memory leak。

以下程式碼動態分配指標建立三個變數，並觀察三個變數的記憶體地址是由小至大增長。證明動態分配記憶體地址向上遞增。
{% highlight c++ linenos %}
int main() {
    int* p1 = new int(10);
    int* p2 = new int(20);
    int* p3 = new int(30);
    cout << "p1 address = " << (long long)p1 << endl;
    cout << "p2 address = " << (long long)p2 << endl;
    cout << "p3 address = " << (long long)p3 << endl;
    delete p1;
    delete p2;
    delete p3;
    p1 = nullptr;
    p2 = nullptr;
    p3 = nullptr;
    return 0;
}
{% endhighlight %}
```
執行結果
p1 address = 105553116315664
p2 address = 105553116315680
p3 address = 105553116315696
```

### 尚未使用區域

變數在Stack區段中，地址增長的方向是向下遞減，變數在Heap區段中，地址增長的方向是向上遞增，未避免Stack與Heap的記憶體地址互相交疊，中間有一個區域是分隔Stack與Heap。