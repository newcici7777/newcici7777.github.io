---
title: 動態規劃背包
date: 2025-08-08
keywords: java, Dynamic programming, DP
---
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
    int[][] rtn = new int[num + 1][packpackW + 1];
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
          // 上一次的價錢 value[i - 1]
          int beforeValue = dp[i - 1][j];
          //背包重量j減物品重量weight[i - 1] = 剩餘重量
          //上一次 剩餘重量的價錢 dp[i - 1][j - weight[i - 1]]
          //物品的價錢 + 上一次 剩餘重量的價錢
          int curValue = value[i - 1] +
              dp[i - 1][j - weight[i - 1]];
          if (curValue > beforeValue) {
            dp[i][j] = curValue;
            rtn[i][j] = 1;
          } else {
            dp[i][j] = beforeValue;
          }
          // 比較大小
         dp[i][j] = Math.max(
             // 上一次的價錢
             dp[i - 1][j], value[i - 1]
             // 與 這次能放入背包
                 // 上一次   剩餘重量
             + dp[i - 1][j - weight[i - 1]]);
       }
      }
    }
    for (int[] arr: dp) {
      System.out.println(Arrays.toString(arr));
    }
    int i = dp.length - 1;
    int j = dp[0].length - 1;
    while (i > 0 && j > 0) {
      if (rtn[i][j] == 1) {
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