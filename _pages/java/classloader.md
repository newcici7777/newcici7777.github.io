---
title: Classloader類別載入
date: 2025-06-05
keywords: java, class loader
---
## 什麼時候會載入類別？
使用到靜態屬性、使用new建立物件。



## 類別載入物件

Class Loader載入類別資訊

類別載入至記憶體，1.會在metaspace建立metadat，裡面包含有Dog類別所有屬性、方法、靜態變數…等等，2.接著在Heap建立一個Class物件，裡面也有屬性、方法、建構子…等等，但實際都是指向metadata的資料，metadata與Class物件只會有一個，生命周期從被載入至記憶體至程序結束，metadata是c++的結構，並不是物件。

![img]({{site.imgurl}}/java/obj_model1.png)

## 靜態變數、靜態區塊載入順序

### 靜態區塊
類別載入的時候，會在Metaspace中建立metadata，裡面有一個clinit()，靜態區塊的程式碼會以inline的方式合併在clinit()函式中。

```
.class 檔 → ClassLoader 載入類別
       ↓
   建立 metadata（Metaspace） 和 java.lang.Class（Heap）
       ↓
   Linking（連接）
     ├── Verification（驗證）
     ├── Preparation（配置 static 區域預設值）
     └── Resolution 
       ↓
Initialization（執行 clinit）
  → static 區塊與 static 欄位被初始化
```

|Linking階段|描述|
|:-----|:-------------|
|Verification|確保 class 檔案格式正確、邏輯正確（例如 stack 不會溢出，方法合法)|
|Preparation|分配 static 欄位的空間，設初始值（int 為 0、Object 為 null）不執行 static 區塊|
|Resolution| 把 symbolic reference（如 Lcom/example/Foo;）轉換為實際reference（即記憶體指標）|

Initialization : static 區塊與 static 欄位被初始化，例如: static int var = 23