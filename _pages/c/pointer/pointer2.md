---
title: 指標2
date: 2025-07-27
keywords: C++, C, pointer 
---
## 基本型態指標
基本型態(int,short,byte,long,long long, unsigned long, char, float, double ...)都有對映的指標類型(int * ,short * ,byte * ,long * ,long long * , unsigned long * , char * , float * , double * ...)。

指標是一個類型，指標名可當作是變數名，存放的值是記憶體位址。<br>

語法<br>
```
指標類型 指標名 = 記憶體位址
```

## 指標記憶體位址
假設有一個int資料型態的變數i，占用記憶體4 byte，變數i占的開始位址0x00000008至結束位址0x0000000B，總共占4Byte。<br>
{% highlight c++ linenos %}
int i = 55;
{% endhighlight %}

有一個`int * `指標類型，指標名是`p`，存放的值是i變數的記憶體位址，使`&`取地址運算子放在i變數前，可以取出i變數的記憶體位址。
{% highlight c++ linenos %}
int * p = &i;
{% endhighlight %}

不管是int * 指標或其它類型的指標，存放記憶體的大小為8 byte，p指標存放的開始位址0x00000011至結束位址0x00000018，總共占8byte。<br>
此時指標p存放的是，變數i的「開始」記憶體位址0x00000008。<br>

<table class="custom-table">
	<tr><th width="10%">記憶體開始與結束</th><th width="45%">記憶體位址</th><th width="45%">占用記憶體位址範圍</th></tr>
	<tr><td>結束</td><td>0xFFFFFFFF</td><td rowspan="4"></td></tr>
	<tr><td rowspan="26">&#8593;</td><td>......</td></tr>
	<tr><td>0x0000001A</td></tr>	
	<tr><td>0x00000019</td></tr>	
	<tr><td>0x00000018</td><td rowspan="8">0x00000008</td></tr>	
	<tr><td>0x00000017</td></tr>	
	<tr><td>0x00000016</td></tr>	
	<tr><td>0x00000015</td></tr>	
	<tr><td>0x00000014</td></tr>	
	<tr><td>0x00000013</td></tr>
	<tr><td>0x00000012</td></tr>
	<tr><td>0x00000011</td></tr>
	<tr><td>0x00000010</td><td rowspan="4"></td></tr>
	<tr><td>0x0000000E</td></tr>
	<tr><td>0x0000000D</td></tr>
	<tr><td>0x0000000C</td></tr>
	<tr><td>0x0000000B</td><td rowspan="4">int i = 55;</td></tr>
	<tr><td>0x0000000A</td></tr>
	<tr><td>0x00000009</td></tr>
	<tr><td>0x00000008</td></tr>
	<tr><td>0x00000007</td><td rowspan="8"></td></tr>
	<tr><td>0x00000006</td></tr>
	<tr><td>0x00000005</td></tr>
	<tr><td>0x00000004</td></tr>
	<tr><td>0x00000003</td></tr>
	<tr><td>0x00000002</td></tr>
	<tr><td>0x00000001</td></tr>
	<tr><td>開始</td><td>0x00000000</td></tr>
</table>


