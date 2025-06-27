---
title: 陣列
date: 2025-06-04
keywords: Java, array
---
陣列裡面放是相同類型的物件，一旦初始化或設定大小，就不能再更改陣列大小。
## 同時初始化與建立陣列
一開始就初始化陣列中的值。
{% highlight java linenos %}
char[] arr1 = {'H', 'e', 'l', 'l', 'o'};
int[] arr2 = {0, 1, 2, 3};
int[] arr3 = new int[]{1, 2, 3};
{% endhighlight %}

## 建立陣列，但並未初始化。
先設定陣列記憶體大小，之後再分配陣列的值。
{% highlight java linenos %}
int arr[] = new int[3];
arr[0] = 1;
arr[1] = 2;
arr[2] = 3;
{% endhighlight %}

\[\]可放在類型後面，也可以放在變數後面，擇一放置。
{% highlight java linenos %}
int[] arr = new int[3];
{% endhighlight %}

## 字元陣列與print
印出字元陣列就是印出字串。
{% highlight java linenos %}
char arr[] = new char[3];
arr[0] = 'A';
arr[1] = 'B';
arr[2] = 'C';
System.out.println(arr);
{% endhighlight %}