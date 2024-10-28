---
title: 記憶體配置
date: 2024-05-06
keywords: c++, 記憶體佈局
---


### 記憶體起始與結束位址。
記憶體開始位址由下表最下方開始，記憶體結束位址在最上方。


<table class="custom-table">
  <thead>
    <tr>
      <th align="center" width="10%">開始與結束</th>
      <th align="left" width="10%">位址高低</th>
      <th align="left">位址</th>
    </tr>  
  </thead>
  <tbody>
    <tr>
        <td>結束</td>
        <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
          高
        </td>
        <td>0xFFFFFFFF</td>
    </tr>
    <tr><td></td><td>0xFFFFFFFE</td></tr>
    <tr><td></td><td>0xFFFFFFFD</td></tr>
    <tr><td></td><td>0xFFFFFFFC</td></tr>
    <tr>
      <td></td>
      <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
          <span style="font-size: 20pt">&#8593;</span>
      </td>
      <td>......</td></tr>
    <tr><td></td><td>......</td></tr>
    <tr><td></td><td>......</td></tr>
    <tr><td></td><td>......</td></tr>
    <tr>
      <td></td>
      <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
          低
      </td>
      <td>0x00000004</td></tr>
    <tr><td></td><td>0x00000002</td></tr>
    <tr><td></td><td>0x00000001</td></tr>
    <tr><td>開始</td><td>0x00000000</td></tr>
  </tbody>
</table>

### 記憶體區段

記憶體區段根據位址由高到低分別為Kernel, Stack, 尚位使用區域, Heap, bass, data, code。

<table class="custom-table">
  <thead>
    <tr>
      <th align="center" width="5%">位址高低</th>
      <th align="left" width="30%">記憶體區塊</th>
      <th align="left" width="5%">位址成長方向</th>
      <th align="left">儲存項目</th>
    </tr>    
  </thead>
  <tbody>
    <tr>
      <td>高</td>
      <td>Kernel</td>
      <td></td>
      <td>作業系統核心</td>
    </tr>
    <tr>
      <td rowspan="5" style="vertical-align: middle;">
          <span style="font-size: 10pt">&#8593;</span>
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
      <td>動態配置記憶體</td>
    </tr>
    <tr>
      <td>bss segment</td>
      <td></td>
      <td>未初始化全域變數, 靜態變數</td>
    </tr>
    <tr>
      <td>data segment</td>
      <td></td>
      <td>已初始化全域變數, 靜態變數</td>
    </tr>
    <tr>
      <td>低</td>    
      <td>code segment</td>
      <td></td>
      <td>常數與程式執行檔</td>
    </tr>
  </tbody>                
</table>

#### 作業系統核心

處理cpu記憶體Devices與應用程式運作。

#### stack(堆疊) 儲存區域變數的記憶體區塊

儲存區域變數與函式參數與函式傳回值，記憶體大小只有8M，記憶體位址成長的方向是向下成長。

#### Heap(堆積) 儲存動態配置變數的記憶體區塊

儲存由動態配置(new與malloc)產生的變數，記憶體大小取決電腦實體記憶體大小(可能8GB或更大)，記憶體位址成長的方向是向上成長。

#### bss segment 記憶體區塊

儲存未初始化全域變數與靜態變數。

#### data segment 記憶體區塊

儲存已初始化全域變數與靜態變數。

#### code segment 記憶體區塊

儲存常數與程式執行檔。

### 變數記憶體位址

{% highlight c++ linenos %}
#include <stdio.h>
const int global_x = 1;  // 儲存於 code segment(常數)
int global_y = 1;        // 儲存於 data segment(已初始化全域變數）
int global_z;            // 儲存於 bss(未初始全域變數)
int fun1(int param1) {	 // 儲存於 stack (函式參數)
	return param1; // 儲存於 stack (函式傳回值)
}
int main() {
  const static int x = 1; // 儲存於 code segment(常數)
  static int y = 1;       // 儲存於 data segment(已初始化靜態變數）
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
* 儲存在Stack的變數，變數離開有效範圍(Scope)後，會由系統自動回收記憶體位址。
* Stack記憶體容量8M。
* 記憶體位址向下成長。
* 不會memory leak。

以下程式碼在函式中建立三個變數，並觀察三個變數的記憶體位址是由大至小遞減。證明記憶體位址向下成長。

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
* 儲存動態配置(new與malloc)產生的變數，由程式設計師手動回收記憶體位址，或待主程式生命周期結束後被系統回收記憶體位址。
* Heap記憶體容量取決於電腦的記憶體大小。
* 記憶體位址向上遞增。
* 會memory leak。

以下程式碼動態分配建立三個變數，並觀察三個變數的記憶體位址是由小至大增長。證明動態分配記憶體位址向上成長。
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

變數在Stack記憶體區塊，位址增長的方向是向下，變數在Heap記憶體區塊，位址增長的方向是向上，未避免Stack與Heap的記憶體位址成長時互相交疊，中間有一個區域是分隔Stack與Heap。