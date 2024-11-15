---
title: 記憶體間隔計算
date: 2024-09-16
keywords: c++, memory spacing problem 
---

Prerequisites:

- [數線間隔](https://www.youtube.com/watch?v=CM1CMYDfNMw&list=PLddNrqlSgts9RWyAxWRHXKtiTAlEKLmqF&index=2)  
- [間隔問題](https://www.youtube.com/watch?v=e1RSUXVSd0o&list=PLddNrqlSgts9RWyAxWRHXKtiTAlEKLmqF&index=7)
- [數線上兩點的距離](https://www.youtube.com/watch?v=MSZb0vOKzQI&list=PLddNrqlSgts8ZYFZcta4ju_oyoiaA_ncr&index=1)

## 計算間隔1

- [xml][1]

指標指向0x00000001，若要指向0x00000005，請問指標要移動幾格？

![img]({{site.imgurl}}/pointer/move2.jpg)

假設指標在0x00000005的位址，計算0x00000005到0x00000001之間有幾個間隔，不包含0x00000005

答案:0x00000005 - 0x00000001 = 4個間隔

|0x00000001|0x00000002|0x00000003|0x00000004|0x00000005|
|||||^|

## 計算間隔2

指標指向0x00000001，若要指向0x00000011，請問指標要移動幾格？

![img]({{site.imgurl}}/pointer/move1.jpg)

把位址全變成10進位，比較好計算。

假設指標在11的位址，計算11到01之間有幾個間隔，不包含11

答案: 11 - 01 = 10個間隔

|位址|01|02|03|04|05|06|07|08|09|10|11|12|13|14|15|16|17
|xml|`<`|n|a|m|e|`>`|C|i|c|i|`<`|`/`|n|a|m|e|`>`|
|指標|||||||||||^|||||||


以下10個間隔，只想取出Cici四個間隔，要如何取？

10個間隔 - 2個括號`<>` - `name`4個字元 = 4

|位址|01|02|03|04|05|06|07|08|09|10|
|xml|`<`|n|a|m|e|`>`|C|i|c|i|
|指標|||||||x|x|x|x|

## 計算間隔3

- [刪除左邊的字元][2]

|位址|01|02|03|04|05|06|07|08|09|10|
|字串|y|y|y|y|o|o|o|o|y|y|
|str指標|str | | | | | | | | | |
|p指標| | | | |p| | | | | |

計算p指標位址與str指標位址，不包含p指標，總共有多少間隔？

答案:5 - 1 = 4個間隔

`yyyyooooyy`字串長度為10，扣掉最左邊yyyy，只要右邊ooooyy的6個字元，數學公式如何寫？

步驟1: strlen(`yyyyooooyy`) = 10 ，字串長度為10

步驟2: 取出p指標與str指標之間有多少間隔，不含p指標，p(5) - str(1) = 4個間隔

步驟3: 步驟1答案(10) - 步驟2答案(4) = 6個字元 

## 指標移動間隔

- [刪除中間字串][3]

|位址|01|02|03|04|05|06|07|
字串|1|2|3|z|z|z|0|
||:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|移動前|find| | || | | |
|移動後|| | |find+3| | | |

find的位址在01

find + 3的意思是，find指標往右移動3格，移到04的位址。


[1]: {% link _pages/c/string/xml.md %}
[2]: {% link _pages/c/string/delstr.md %}#刪除左邊的字元
[3]: {% link _pages/c/string/delstr.md %}#刪除中間字串