---
title: list
date: 2026-03-12
keywords: Python, list
---
## 語法
使用[ ] 方括號代表list。<br>

### 空 list
list() 或 [] 都是宣告空的list。<br>

## 元素
用逗號分隔list中的元素。<br>

## list記憶體位址
Prerequisites:

- [id][1]
- [C++ 指標陣列][2]

先前在id的文章中提到，英文大小寫字母、數字0-9、底線，為了節省記憶體空間，Python都會指向相同記憶體位址。<br>

Python list的記憶體位址分配跟C++、Java不一樣。<br>

由以下程式碼可以看出x變數、`nums[0]`、`nums[1]`、`nums[2]`、`nums[3]`全指向同一個記憶體位址。<br>

|0|1|2|3|
|:--------:|:--------:|:--------:|:--------:|
|4370549520|4370549520|4370549520|4370549520|

{% highlight python linenos %}
nums = [1, 1, 1, 1]
x = 1
print("address of nums = ", id(nums))
print("address of nums[0] = ", id(nums[0]))
print("address of nums[1] = ", id(nums[1]))
print("address of nums[2] = ", id(nums[2]))
print("address of nums[3] = ", id(nums[3]))
print("address of x = ", id(x))
{% endhighlight %}
```
address of nums =  4360423744
address of nums[0] =  4370549520
address of nums[1] =  4370549520
address of nums[2] =  4370549520
address of nums[3] =  4370549520
address of x =  4370549520
```

用C++的觀點是，建立一個指標陣列，指標就是記憶體位址，陣列中儲存的是記憶體位址。<br>
{% highlight c++ linenos %}
int* array[4];
int var1 = 1;
array[0] = &var1;
array[1] = &var1;
array[2] = &var1;
array[3] = &var1;
{% endhighlight %}

記憶體位址4370549520儲存「1」的數值。
```
4370549520 → 1
```

指標陣列儲存的是記憶體位址，陣列中的pointer都是1的address。
```
 4360423744
[    0    ][    1    ][    2    ][    3    ]
[ pointer ][ pointer ][ pointer ][ pointer ]
[   &1    ][   &1    ][   &1    ][   &1    ]
[ 4370549520 ][ 4370549520 ][ 4370549520 ][ 4370549520 ]
```

而nums則是指向指標陣列`[0]`的記憶體位址4360423744，指標陣列`[0]`儲存的內容是1的記憶體位址4370549520。<br>

[1]: {% link _pages/python/id.md %}
[2]: {% link _pages/c/array/arrayOfPointers.md %}