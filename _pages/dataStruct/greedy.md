---
title: 貪心演算法
date: 2025-08-09
keywords: java, greedy
---
貪心演算法，每一次都盡可能「挑最大」。<br>

## 零錢組合
只有以下二種硬幣，請問可以湊成11元，最少的硬幣組合方式。
```
5元 1元
```

貪心演算法，每一次挑最大。
### 第1次
選5元，挑最大硬幣。

11 - 5 = 6

### 第2次
選5元，挑最大硬幣。

6 - 5 = 1

### 第3次
不能選5元，因為餘額只剩下1元，只能挑1元。

1 - 1 = 0

所以總共使用2個5元硬幣，與1個1元硬幣。

## 零錢組合2
只有以下二種硬幣，請問可以湊成11元，最少的硬幣組合方式。
```
5元 3元
```

貪心演算法，每一次挑最大。
### 第1次
選5元，挑最大硬幣。

11 - 5 = 6

### 第2次
選5元，挑最大硬幣。

6 - 5 = 1

### 第3次
有餘額，而且不夠減，策略有問題，backtracking退回前一個步驟。

1 - 3 = 不夠減

### 回到第2次
選3元

6 - 3 = 3

### 第3次
選3元

3 - 3 = 0

總共使用1個5元硬幣，與2個3元硬幣。

## 基地台涵蓋問題
基地台涵蓋的所有地區:<br>
```
台北, 台東, 彰化, 花蓮, 高雄, 澎湖, 苗栗, 宜蘭
```

各個基地台涵蓋的範圍如下:

|基地台編號|涵蓋範圍|
|0|台北 台東 彰化|
|1|花蓮 高雄 澎湖|
|2|苗栗 宜蘭 台北|
|3|苗栗 高雄|
|4|澎湖 宜蘭|

請問可以涵蓋所有地區的基地台組合有那些？<br>
答案是:0號、1號、2號。<br>

## 程式步驟
### 第1次
統計涵蓋數量。

|基地台編號|涵蓋範圍|數量|
|0|台北 台東 彰化|3|
|1|花蓮 高雄 澎湖|3|
|2|苗栗 宜蘭 台北|3|
|3|苗栗 高雄|2|
|4|澎湖 宜蘭|2|

找出上面涵蓋數量「最大」的基地台編號，基地台0號、1號、2號都是數量3，排序由編號最小的開始，此次涵蓋數量最大的基地台是0，把0號儲存在清單。<br>

|基地台編號|涵蓋範圍|數量|
|0|台北 台東 彰化|3|

list清單
```
0
```

基地台0號的涵蓋範圍台北、台東、彰化。<br>
以下所有涵蓋地區若在基地台0號的範圍內，則刪掉。<br>

~~台北~~, ~~台東~~, ~~彰化~~, 花蓮, 高雄, 澎湖, 苗栗, 宜蘭


刪除完變這樣:
```
花蓮, 高雄, 澎湖, 苗栗, 宜蘭
```

### 第2次
基地台涵蓋地區
```
花蓮, 高雄, 澎湖, 苗栗, 宜蘭
```

|基地台編號|涵蓋範圍|數量|
|0|台北 台東 彰化|0|
|1|花蓮 高雄 澎湖|3|
|2|苗栗 宜蘭 台北|2|
|3|苗栗 高雄|2|
|4|澎湖 宜蘭|2|

找出上面涵蓋數量「最大」的基地台編號，此次涵蓋數量最大的基地台是1，把1號儲存在清單。<br>

list清單
```
0, 1
```

基地台1號的涵蓋範圍花蓮 高雄 澎湖。<br>
以下所有涵蓋地區若在基地台1號的範圍內，則刪掉。<br>

~~花蓮~~, ~~高雄~~, ~~澎湖~~, 苗栗, 宜蘭


刪除完變這樣:
```
苗栗, 宜蘭
```

### 第3次
基地台涵蓋地區
```
苗栗, 宜蘭
```

|基地台編號|涵蓋範圍|數量|
|0|台北 台東 彰化|0|
|1|花蓮 高雄 澎湖|0|
|2|苗栗 宜蘭 台北|2|
|3|苗栗 高雄|1|
|4|澎湖 宜蘭|1|

找出上面涵蓋數量「最大」的基地台編號，此次涵蓋數量最大的基地台是2，把2號儲存在清單。<br>

list清單
```
0, 1, 2
```

基地台2號的涵蓋範圍苗栗 宜蘭 台北。<br>
以下所有涵蓋地區若在基地台2號的範圍內，則刪掉。<br>

~~苗栗~~, ~~宜蘭~~


刪除完變這樣:
```

```

刪除變空，就可以結束。

## 程式碼
{% highlight java linenos %}
public class BaseStation {
  public static void main(String[] args) {
    String[] area = new String[]{"台北", "台東", "彰化", "花蓮", "高雄","澎湖","苗栗","宜蘭"};
    List areaList = new ArrayList(Arrays.asList(area));
    List<List<String>> baseStation = new ArrayList<>();
    baseStation.add(0, Arrays.asList("台北", "台東", "彰化"));
    baseStation.add(1, Arrays.asList("花蓮", "高雄", "澎湖"));
    baseStation.add(2, Arrays.asList("苗栗", "宜蘭", "台北"));
    baseStation.add(3, Arrays.asList("苗栗", "高雄"));
    baseStation.add(4, Arrays.asList("澎湖","宜蘭"));
    List<Integer> result = new ArrayList<>();
    // 刪到所有涵蓋範圍為0，才停止。
    while (areaList.size() > 0) {
      // 此次最大涵蓋的基地台編號
      int maxKey = -1;
      // 基地台涵蓋數量
      int maxCnt = 0;
      // 遍歷所有基地台
      for (int i = 0; i < baseStation.size(); i++) {
        // 基地台涵蓋地區清單
        List subList = baseStation.get(i);
        // 涵蓋地區數量
        int count = contains(areaList, subList);
        // 此基地台涵蓋數量是否為最大?
        if (count > maxCnt) {
          maxKey = i;
          maxCnt = count;
        }
      }
      // 所有地區包含在「最大涵蓋的基地台」地區範圍，則刪除
      containsRemove(areaList, baseStation.get(maxKey));
      // 儲存最大涵蓋的基地台
      result.add(maxKey);
    }
    System.out.println("result=" + result);
  }
  // 涵蓋地區數量計算
  public static int contains(List<String> areaList,List<String> subList) {
    int count = 0;
    for (int i = 0; i < areaList.size(); i++) {
      if (subList.contains(areaList.get(i))) {
        count++;
      }
    }
    return count;
  }
  // 所有地區包含在「最大涵蓋的基地台」地區範圍，則刪除
  public static void containsRemove(List<String> areaList,List<String> subList) {
    for (int i = 0; i < subList.size(); i++) {
      if (areaList.contains(subList.get(i))) {
        areaList.remove(subList.get(i));
      }
    }
  }
}
{% endhighlight %}
```
result=[0, 1, 2]
```

