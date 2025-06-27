---
title: Arrays方法與System方法
date: 2025-06-26
keywords: java, Arrays
---
## Arrays.toString()
傳回字串。

需要搭配`System.out.println()`，才能印出陣列內容，因為此方法只是傳回字串，沒有印出的功能。

{% highlight java linenos %}
int[] arr = {100, 20, 64, 1, 98};
System.out.println(Arrays.toString(arr));
{% endhighlight %}
```
[100, 20, 64, 1, 98]
```

## Arrays.copyOf()
複製陣列。

### 語法
語法
```
目標陣列 = Arrays.copyOf(要複製的來源陣列，要複製的大小);
```

{% highlight java linenos %}
int[] arr = {100, 20, 64, 1, 98};
int[] dest = Arrays.copyOf(arr, arr.length);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
[100, 20, 64, 1, 98]
```

### 複製大小超出陣列大小
#### 基本型態
會根據陣列類型超出的索引，會指派預設值，以下範例陣列是int基本型態，預設值為0。
{% highlight java linenos %}
int[] arr = {100, 20, 64, 1, 98};
int[] dest = Arrays.copyOf(arr, arr.length + 5);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
[100, 20, 64, 1, 98, 0, 0, 0, 0, 0]
```

#### 類別
以下範例陣列是Integer，超出的索引，設為null。
{% highlight java linenos %}
Integer[] arr = {100, 20, 64, 1, 98};
Integer[] dest = Arrays.copyOf(arr, arr.length + 5);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
[100, 20, 64, 1, 98, null, null, null, null, null]
```

### 大小設為0
0是建立空的陣列。
{% highlight java linenos %}
Integer[] arr = {100, 20, 64, 1, 98};
Integer[] dest = Arrays.copyOf(arr, 0);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
[]
```
### 不能設為負數
以下程式碼，會產生例外。
{% highlight java linenos %}
Integer[] arr = {100, 20, 64, 1, 98};
Integer[] dest = Arrays.copyOf(arr, -1);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
Exception in thread "main" java.lang.NegativeArraySizeException: -1
```

## System.arraycopy()
語法
```
System.arraycopy(
 來源陣列, 
 從來源陣列那一個位置開始複製, 
 目標陣列, 
 複製到目標陣列的那一個位置開始新增, 
 要複製多少個);
```

以下程式碼從來源陣列第0個位置開始複製，複製到目標陣列的第0個位置開始新增，複製5個。
{% highlight java linenos %}
int[] source = {100, 20, 64, 1, 98};
int [] dest = new int[source.length];
System.arraycopy(source, 0, dest, 0, source.length);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
[100, 20, 64, 1, 98]
```

### 複製大小超出來源陣列大小
超出「來源陣列」大小會產生例外，例外訊息的意思是來源陣列只有5個，沒有100個。<br>
跟Arrays.copyOf()不同，Arrays.copyOf()超出來源陣列大小，會自動產生預設值。
{% highlight java linenos %}
int[] source = {100, 20, 64, 1, 98};
int [] dest = new int[source.length];
System.arraycopy(source, 0, dest, 0, 100);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: arraycopy: last source index 100 out of bounds for int[5]
```

### 複製大小自訂
複製大小設為來源陣列大小-1<br>
從執行結果發現，最後面會多出一個0。<br>
int基本型態，預設值為0，若是類別，預設值是null。
{% highlight java linenos %}
int[] source = {100, 20, 64, 1, 98};
int [] dest = new int[source.length];
System.arraycopy(source, 0, dest, 0, source.length - 1);
System.out.println(Arrays.toString(dest));
{% endhighlight %}
```
[100, 20, 64, 1, 0]
```

## System.currentTimeMillis()
取得現在時間到1970-01-01的毫秒，通常用來計算程式執行時間。<br>
1秒 = 1000毫秒<br>
0.5秒 = 500毫秒<br>
0.1秒 = 100毫秒<br> 
0.001秒 = 1毫秒<br>
請自行換算，以下執行結果是以毫秒為單位。
{% highlight java linenos %}
long startT = System.currentTimeMillis();
    // 中間是要執行的程式碼
long endT = System.currentTimeMillis();
System.out.println("執行時間 =  " + (endT - startT));
{% endhighlight %}

## System.exit(0)
退出程式。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println("print1");
    System.exit(0);
    System.out.println("print2");
  }
}
{% endhighlight %}
```
print1
```

## Arrays.fill()
把陣列中的值全設為自訂預設值。

語法
```
Arrays.fill(陣列, 自訂預設值);
```

{% highlight java linenos %}
int[] arr = {1, 2, 3};
Arrays.fill(arr, 100);
System.out.println(Arrays.toString(arr));
{% endhighlight %}
```
[100, 100, 100]
```

## Arrays.equals()
比較2個陣列中的值是否相同。
{% highlight java linenos %}
int[] arr1 = {1, 2, 3};
int[] arr2 = {1, 2, 5};
boolean bool = Arrays.equals(arr1, arr2);
System.out.println(bool);
{% endhighlight %}
```
false
```
## Arrays.asList
把參數轉成List。
{% highlight java linenos %}
List<Integer> list = Arrays.asList(4, 5, 6);
System.out.println(list);
{% endhighlight %}
```
[4, 5, 6]
```

## Arrays.binarySearch
要先排好序，才能使用binarySearch()方法，會傳回找到的索引位置。
{% highlight java linenos %}
int[] arr = {-1, 5, 1, 10, 0, 7};
// 由小到大
Arrays.sort(arr);
System.out.println("排序後 : " + Arrays.toString(arr));
int index = Arrays.binarySearch(arr, 7);
System.out.println(index);
{% endhighlight %}

### 找不到
找不到，會傳回負的應該插入的下一個位置。
{% highlight java linenos %}
int[] arr = {-1, 5, 1, 10, 0, 7};
// 由小到大
Arrays.sort(arr);
System.out.println("排序後 : " + Arrays.toString(arr));
int index = Arrays.binarySearch(arr, 3);
System.out.println(index);
{% endhighlight %}
```
排序後 : [-1, 0, 1, 5, 7, 10]
-4
```

要找尋3，但3沒在陣列中，會傳回負號(-)應該插入的位置(3) \+ 1 = -4
```
 0   1  2  3  4  5
[-1, 0, 1, 5, 7, 10]
           ↑
```




