---
title: Classloader類別載入
date: 2025-06-05
keywords: java, class loader, metadata, metaspace
---
## metadata
在Metaspace(Native memory)中，還會存放metadata，儲存類別的所有屬性、靜態或非靜態方法、建構子、靜態變數、靜態區塊，它是屬於C++結構，每一個類別只有一個metadata。

![img]({{site.imgurl}}/java/memory_model.png)

### Field info（非靜態屬性)
### method info
包含建構子、靜態方法、非靜態方法。
```
├─ Method info（包括靜態、非靜態方法和建構子）
│    ├─ Method: <init>(int) 建構子
│    └─ Method: toString(), etc. 
```

包含靜態區塊`<clinit>()`


```
- Method Name         → "doSomething" 
- Method Descriptor   → "(I)Ljava/lang/String;" 
- Access Flags        → public static 
- Code Pointer        → 指向 compiled code 
- Max Stack Size      → 10            
- Local Variable Table→ 參數與區域變數記錄表
- Exception Table     → try-catch 區塊資訊
- Line Number Table   → source 對映表
- Runtime Annotations → @Override, etc.
```


### vtable
可以override的方法。

也包含建構子，靜態方法不能覆寫，就沒有在裡面。


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