---
title: 十進位
date: 2025-06-23
keywords: 十分位,百分位,千分位
---
## 10進位系統
![img]({{site.imgurl}}/java/decimal.png)

左移1位乘10，代表進1位。

右移1位除10，代表退1位。

![img]({{site.imgurl}}/math/tenth.png)

範例1:

個位數1，往左移一位，乘10，代表進1位，變成十位數。
```
1
↓
左移1位
↓
10
```

範例2:

個位數1，往右移一位，除10，代表退1位，變成十分位。
```
1
↓
右移1位
↓
0.1
```

## 科學記號法
科學記號法可以使用e或E，若是float，後面要加上f或F。

1e1 = $ 1 \times 10^{1} = 1 \times 10$ 

1E2 = $ 1 \times 10^{2} = 1 \times 10 \times 10$

1e3 = $ 1 \times 10^{3} = 1 \times 10 \times 10 \times 10$

1e-1 = $ 1 \times 10^{-1} = 1 \times 0.1 = 1 \times \frac{1}{10} =  1\div10$

1E-2 = $ 1 \times 10^{-2} = 1 \times 0.01 = 1 \times \frac{1}{10} \times \frac{1}{10} =  1 \times \frac{1}{100} = 1 \div 10 \div 10$

1E-3 = $ 1 \times 10^{-3} = 1 \times 0.001 = 1 \times \frac{1}{10} \times \frac{1}{10} \times \frac{1}{10} =  1 \times \frac{1}{1000} = 1 \div 10 \div 10 \div 10$