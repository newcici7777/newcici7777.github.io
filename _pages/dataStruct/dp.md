---
title: 動態規劃背包
date: 2025-08-08
keywords: java, Dynamic programming, DP
---
背包問題在於如何在有限的背包承重能力下，裝入最大的物品價值。

## 物品的重量與價錢

|物品|重量|
|:------:|:-----:|
|第1個物品|1kg|
|第2個物品|4kg|
|第3個物品|3kg|

|物品|價錢|
|:------:|:-----:|
|第1個物品|1500|
|第2個物品|3000|
|第3個物品|2000|

## 預設為0
每一列(row)，代表第幾個物品。<br>
每一欄(column)，代表背包可放重量。<br>
第0個物品，代表沒有東西，不管背包承重多少公斤，全部都是0。<br>

<table>
	<tr>
		<th>物品\背包重量</th>
		<th>0kg</th>
		<th>1kg</th>
		<th>2kg</th>
		<th>3kg</th>
		<th>4kg</th>
	</tr>
	<tr>
		<td>第0個物品</td>
		<td bgcolor="#FF8000">0</td>
		<td bgcolor="#FF8000">0</td>
		<td bgcolor="#FF8000">0</td>
		<td bgcolor="#FF8000">0</td>
		<td bgcolor="#FF8000">0</td>
	</tr>
	<tr>
		<td>第1個物品</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>第2個物品</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>第3個物品</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>			
</table>

當背包承重重量為0kg，不管是第幾個物品都不能放入背包，所以填0。<br>

<table>
	<tr>
		<th>物品\背包重量</th>
		<th>0kg</th>
		<th>1kg</th>
		<th>2kg</th>
		<th>3kg</th>
		<th>4kg</th>
	</tr>
	<tr>
		<td>第0個物品</td>
		<td bgcolor="#FF8000">0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
	</tr>
	<tr>
		<td>第1個物品</td>
		<td bgcolor="#FF8000">0</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>第2個物品</td>
		<td bgcolor="#FF8000">0</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>第3個物品</td>
		<td bgcolor="#FF8000">0</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>			
</table>

## 第一個物品
程式是從第1個物品，與背包承重能力1kg開始，不是第0個物品跟背包0kg開始。<br>
第1個物品重量是1kg，價值是1500元。<br>
第1個物品重量1kg，代表背包1至4公斤，都可放入，而且1500大於上一列(row)中1至4公斤的0元。<br>
所以在1至4公斤，都填入物品的價錢。<br>

<table>
	<tr>
		<th>物品\背包重量</th>
		<th>0kg</th>
		<th>1kg</th>
		<th>2kg</th>
		<th>3kg</th>
		<th>4kg</th>
	</tr>
	<tr>
		<td>第0個物品</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
	</tr>
	<tr>
		<td>第1個物品</td>
		<td>0</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#FF8000">1500</td>
	</tr>
	<tr>
		<td>第2個物品</td>
		<td>0</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>第3個物品</td>
		<td>0</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>			
</table>

## 第二個物品
第2個物品重量是4kg，價值是3000元。<br>
物品重量4kg，代表背包1至3公斤，都不能放入。<br>
不能放入的情況下，只能填上一列(row)的價錢，上一列代表可以放入的最大價值。<br>
背包承重4公斤，可以放入第二個物品，表格填上第二個物品價錢3000。<br>

<table>
	<tr>
		<th>物品\背包重量</th>
		<th>0kg</th>
		<th>1kg</th>
		<th>2kg</th>
		<th>3kg</th>
		<th>4kg</th>
	</tr>
	<tr>
		<td>第0個物品</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
	</tr>
	<tr>
		<td>第1個物品</td>
		<td>0</td>
		<td>1500</td>
		<td>1500</td>
		<td>1500</td>
		<td>1500</td>
	</tr>
	<tr>
		<td>第2個物品</td>
		<td>0</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#02DF82">3000</td>
	</tr>
	<tr>
		<td>第3個物品</td>
		<td>0</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
		<td>&nbsp;</td>
	</tr>			
</table>

## 第三個物品
第3個物品重量是3kg，價值是2000元。<br>
物品重量3kg，代表背包1至2公斤，都不能放入。<br>
背包1至2公斤不能放入的情況下，只能填上一列(row)的價錢，上一列代表可以放入的最大價值。<br>
背包承重3公斤，第3物品重量3公斤，而且第三個物品的價錢2000，比上一列1500大，填上第三個物品價錢2000。<br>
背包承重4公斤，背包承重4公斤減第三個物品3公斤，還有1公斤的剩餘重量可裝1公斤的東西。<br>
到上一列(row)，尋找背包承重1公斤的價錢1500。<br>
第三個物品的價格2000 \+ 1公斤(第1個物品)的價格1500，二者相加為3500，大於上一列4公斤3000元的價格，把3500填入。<br>
<table>
	<tr>
		<th>物品\背包重量</th>
		<th>0kg</th>
		<th>1kg</th>
		<th>2kg</th>
		<th>3kg</th>
		<th>4kg</th>
	</tr>
	<tr>
		<td>第0個物品</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0</td>
	</tr>
	<tr>
		<td>第1個物品</td>
		<td>0</td>
		<td>1500</td>
		<td>1500</td>
		<td>1500</td>
		<td>1500</td>
	</tr>
	<tr>
		<td>第2個物品</td>
		<td>0</td>
		<td>1500</td>
		<td>1500</td>
		<td>1500</td>
		<td>3000</td>
	</tr>
	<tr>
		<td>第3個物品</td>
		<td>0</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#FF8000">1500</td>
		<td bgcolor="#02DF82">2000</td>
		<td bgcolor="#FF9797">3500</td>
	</tr>			
</table>

## 公式
物品重量 > 背包承重重量，使用上一列(row)可以放入的最大價值。<br>
{% highlight java linenos %}
// i是第幾個物品 j是背包承重重量
if (weight[i] > j) {
  dp[i][j] = dp[i - 1][j];
}
{% endhighlight %}


### 有剩餘重量
物品重量 <= 背包承重重量:<br>
背包重量 - 物品重量 = 有剩餘重量。<br>
去前一列找剩餘重量的最大價值 \+ 目前物品價值 = 相加價值，若相加價值大於上一列row的最大價值，填入相加價值。<br>
{% highlight java linenos %}
// i是第幾個物品 j是背包承重重量
// restW是剩餘重量
int restW = j - weight[i];
totalValue = value[i] + dp[i-1][restW];
// dp[i-1][j]代表上一列最大價值
if (totalValue > dp[i-1][j]) {
	dp[i][j] = totalValue;
}
{% endhighlight %}

### 沒剩餘重量
背包重量 - 物品重量 = 0 <br>
若物品價值大於上一列row的最大價值，填入物品價值。<br>

{% highlight java linenos %}
// i是第幾個物品 j是背包承重重量
// 物品價值 大於 上一列最大價值
if (value[i] > dp[i-1][j]) {
	dp[i][j] = value[i];
}
{% endhighlight %}

## 顯示最大價值是放入那些物品
設計一個跟dp二維陣列一模一樣大小的二維陣列。<br>
記錄最大價值的i與j。<br>
{% highlight java linenos %}
int[][] mark = new int[num + 1][packpackW + 1];
{% endhighlight %}

存入最大價格時，把`mark[i][j]`設為1。

{% highlight java linenos %}
mark[i][j] = 1;
{% endhighlight %}

因為最大價值一定在最後，所以要二維陣列要由後往前找。
{% highlight java linenos %}
int i = dp.length - 1;
int j = dp[0].length - 1;
{% endhighlight %}

剩餘重量 = 背包的承重重量j 減 物品重量，j要移動到上一列(i--)，剩餘重量j的位置。
{% highlight java linenos %}
while (i > 0 && j > 0) {
  // 找到記錄最大價值的i與j
  if (mark[i][j] == 1) {
    System.out.println("第"+ i + "個物品，由1開始數");
    // 剩餘重量 = 背包的重量j - 物品重量
    j = j - weight[i - 1];
  }
  // 移動到上一列
  i--;
}
{% endhighlight %}

## 完整程式碼
{% highlight java linenos %}
public class backpack {
  public static void main(String[] args) {
    int[] weight = {1, 4, 3}; // 物品重量
    int[] value = {1500, 3000, 2000}; // 物品價錢
    int packpackW = 4; //背包重量，最大4公斤
    int num = weight.length; // 物品數量，要與weight value大小一致。
    // row是物品項目，column是背包重量
    // +1是因為有0個物品，0kg背包重量
    int[][] dp = new int[num + 1][packpackW + 1];
    int[][] mark = new int[num + 1][packpackW + 1];
    // i = 1 從第1個物品開始
    for (int i = 1; i < dp.length; i++) {
      // j = 1 從1公斤開始
      for (int j = 1; j < dp[0].length; j++) {
        // weight[i-1]是因為i從1開始，weight[1]代表是陣列索引為1的物品的重量
        // 但我想從索引為0開始weight[1-1] == weight[0]
        if (weight[i - 1] > j) { // 若物品重量超出背包重量
         // 物品重量塞不進背包，使用上一個可以放下背包重量的價錢
          dp[i][j] = dp[i - 1][j];
       } else { // 物品重量能放入背包
          // 上一列的最大價值
          int beforeValue = dp[i - 1][j];
          //背包重量j減物品重量weight[i - 1] = 剩餘重量
          //上一次 剩餘重量的價錢 dp[i - 1][j - weight[i - 1]]
          //物品的價錢 + 上一次 剩餘重量的價錢
          int curValue = value[i - 1] +
              dp[i - 1][j - weight[i - 1]];
          if (curValue > beforeValue) {
            dp[i][j] = curValue;
            mark[i][j] = 1;
          } else {
            dp[i][j] = beforeValue;
          }
       }
      }
    }
    for (int[] arr: dp) {
      System.out.println(Arrays.toString(arr));
    }
    int i = dp.length - 1;
    int j = dp[0].length - 1;
    while (i > 0 && j > 0) {
      if (mark[i][j] == 1) {
        System.out.println("第"+ i + "個物品，由1開始數");
        j = j - weight[i - 1];
      }
      i--;
    }
  }
}

{% endhighlight %}
```
[0, 0, 0, 0, 0]
[0, 1500, 1500, 1500, 1500]
[0, 1500, 1500, 1500, 3000]
[0, 1500, 1500, 2000, 3500]
第3個物品，由1開始數
第1個物品，由1開始數
```