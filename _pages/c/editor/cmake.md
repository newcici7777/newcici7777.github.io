---
title: Cmake on Mac
date: 2024-04-18
keywords: Cmake,mac,c++
---

前往[下載Cmake](https://cmake.org/download/)  
![img]({{site.imgurl}}/cmake/1.png)  
解壓好放在應用程式中  
![img]({{site.imgurl}}/cmake/2.png) 
打開CMAKE  
按照下圖，選擇Tool->How to Install For Command Line Use   
![img]({{site.imgurl}}/cmake/3.png)  
將以下紅框的部分複製，打開終端機，貼上  
![img]({{site.imgurl}}/cmake/3-1.png)  
![img]({{site.imgurl}}/cmake/3-2.png)  
貼上後再輸入以下指令  
```
$which cmake
```  
再輸入以下指令
```
$cmake --version
```
切記，把版本資訊記起來，如我的版本資訊是cmake version 3.27.5
在你所在.cpp的目錄下建立build目錄  

![img]({{site.imgurl}}/cmake/4.png) 
跟build目錄同階層建立CMakeLists.txt的文件  
![img]({{site.imgurl}}/cmake/5.png)  
在CMakeLists.txt的文件中輸入以下內容 
```
//VERSION編號是之前查詢的
cmake_minimum_required(VERSION 3.27.5)

//第一參數為Project名稱
//Project的版本是1.0.0
project(CPPLessons VERSION 1.0.0)

//第一個參數是Project名稱 第二個參數為所使用的cpp
add_executable(CPPLessons test.cpp Student.cpp)
```
建立檔案，依xcode為例，在工作列按滑鼠右鍵，選"New File"  
![img]({{site.imgurl}}/cmake/6.png)  
依照下圖建立頭文件  
![img]({{site.imgurl}}/cmake/7.png)  
![img]({{site.imgurl}}/cmake/8.png)  
再來建立cpp文件  
![img]({{site.imgurl}}/cmake/9.png)  
記得不要勾選要建立頭文件
![img]({{site.imgurl}}/cmake/10.png)  
在test.cpp輸入以下內容
{% highlight c++ linenos %}
#include <stdio.h>
#include "test.h"
int main() {
  int a[] = {1,2,3,4};
   printf("cmake test");
   int b[] = {7,8,9,100};
   printf("cmake test2");
   return 0;
}
{% endhighlight %}
打開終端機，進入build的目錄  
![img]({{site.imgurl}}/cmake/11.png)  
輸入以下內容(要在build的目錄下做喔)  
```
cmake ..
```
![img]({{site.imgurl}}/cmake/12.png)  
產生以下資訊  
![img]({{site.imgurl}}/cmake/13.png)  
輸入make  
```
make
```
![img]({{site.imgurl}}/cmake/14.png)  
輸入./CPPLessons  
![img]({{site.imgurl}}/cmake/15.png)  
測試成功  
![img]({{site.imgurl}}/cmake/16.png)  
 