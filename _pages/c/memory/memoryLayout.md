---
title: 記憶體配置
date: 2024-05-06
keywords: c++, 記憶體佈局
---


### 記憶體起始與結束位址。
記憶體開始位址由下表最下方開始，記憶體結束位置在最上方。


<table>
  <tr>
      <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
        <span style="font-size: 20pt">高</span>
      </td>
      <td>0xFFFFFFFF</td>
  </tr>
  <tr><td>0xFFFFFFFE</td></tr>
  <tr><td>0xFFFFFFFD</td></tr>
  <tr><td>0xFFFFFFFC</td></tr>
  <tr>
    <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
        <span style="font-size: 30pt">↓</span>
    </td>
    <td>......</td></tr>
  <tr><td>......</td></tr>
  <tr><td>......</td></tr>
  <tr><td>......</td></tr>
  <tr>    <td align="center" rowspan="4" width="5%" style="vertical-align: middle;">
        <span style="font-size: 20pt">低</span>
    </td><td>0x00000004</td></tr>
  <tr><td>0x00000003</td></tr>
  <tr><td>0x00000002</td></tr>
  <tr><td>0x00000001</td></tr>
</table>

### 記憶體區段

<table>
  <tr>
    <td width="5%"></td>
    <td width="30%">Kernel</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Stack</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>尚未使用區域</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>Heap</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>bss segmen</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>data segmen</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>data segmen</td>
    <td></td>
  </tr>                
</table>

|位址增長方向|記憶體區段|儲存項目|
|:---:|:---:|:---|
||Kernel|內核|
|&#8595;|Stack|區域變數| 
||尚未使用區域||
|&#8593;|Heap|指標|
||bss|全域變數|
||data segmen|全域變數|
||code segment|程式執行檔|

#### 內核

處理cpu記憶體Devices與應用程式運作。

#### stack

函式或程式區塊`{}`中區域變數與函式參數與函式返回值的記憶體位址。

#### Heap

動態分配指標記憶體位址

#### bss

未初始化全域變數與靜態變數記憶體位址

#### data segmen

已初始化全域變數與靜態變數記憶體位址

#### code segment

常數與程式執行檔位址


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
* 儲存在Stack的變數，變數離開有效範圍(Scope)後，會由系統自動釋放記憶體位址。
* Stack記憶體容量8M。
* 記憶體位址向下遞減。
* 不會memory leak。

### Heap
* 儲存在Heap的指標，由程式設計師手動釋放記憶體位址，或待主程式生命周期結束後被系統釋放記憶體位置。
* Heap記憶體容量取決於電腦的記憶體大小。
* 記憶體位址向上遞增。
* 會memory leak。
