---
title: Metadata
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

