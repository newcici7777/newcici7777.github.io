---
title: float
date: 2026-03-12
keywords: Python, float
---
浮點數的表示方式:
- 小數點
- 科學記號法

## 小數點
0.5的個位數為0，可以省略0，直接.5。<br>
{% highlight python linenos %}
n1 = 0.5
n2 = .5
print(f"n1 = {n1}, n2 = {n2}")
{% endhighlight %}
```
n1 = 0.5, n2 = 0.5
```

## 科學記號法
科學記號法可以使用e或E。

1e1 = $ 1 \times 10^{1} = 1 \times 10$ 

1E2 = $ 1 \times 10^{2} = 1 \times 10 \times 10$

1e3 = $ 1 \times 10^{3} = 1 \times 10 \times 10 \times 10$

1e-1 = $ 1 \times 10^{-1} = 1 \times 0.1 = 1 \times \frac{1}{10} =  1\div10$

1E-2 = $ 1 \times 10^{-2} = 1 \times 0.01 = 1 \times \frac{1}{10} \times \frac{1}{10} =  1 \times \frac{1}{100} = 1 \div 10 \div 10$

1E-3 = $ 1 \times 10^{-3} = 1 \times 0.001 = 1 \times \frac{1}{10} \times \frac{1}{10} \times \frac{1}{10} =  1 \times \frac{1}{1000} = 1 \div 10 \div 10 \div 10$

例如:<br>

0.21e2 為 $ 0.21 \times 10^{2} = 0.21 \times 10 \times 10 = 21 $

0.21e-2 為$ 0.21 \div 10 \div 10 = 0.0021$

{% highlight python linenos %}
n1 = 0.21e2
n2 = .21e-2
print(f"n1 = {n1}, n2 = {n2}")
{% endhighlight %}
```
n1 = 21.0, n2 = 0.0021
```

## 浮點數最大值 最小值
打開終端機，輸入python3，
```
XXXX@xxxx ~ % python3
Python 3.14.3 (v3.14.3:323c59a5e34, Feb  3 2026, 11:41:37) [Clang 16.0.0 (clang-1600.0.26.6)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
```

再輸入 `import sys` 斷行，再輸入 `sys.float_info`，就會例出最大值與最小值
```
>>> import sys
>>> sys.float_info
sys.float_info(max=1.7976931348623157e+308, max_exp=1024, max_10_exp=308, min=2.2250738585072014e-308, min_exp=-1021, min_10_exp=-307, dig=15, mant_dig=53, epsilon=2.220446049250313e-16, radix=2, rounds=1)
>>> 
```

最大值 max=1.7976931348623157e+308<br>
最小值 min=2.2250738585072014e-308<br>

## 浮點數 近似值
計算機手動算的結果為2.7，但python執行結果並非2.7，而是接近「近似值」
{% highlight python linenos %}
n = 8.1 / 3
print(f"n = {n}")
{% endhighlight %}
```
n = 2.6999999999999997
```

解決近似值使用Decimal，使用前要import Decimal。<br>
{% highlight python linenos %}
from decimal import Decimal
n = 8.1 / 3
print(f"n = {n}")
n2 = Decimal("8.1") / Decimal("3")
print(f"n2 = {n2}")
{% endhighlight %}
```
n = 2.6999999999999997
n2 = 2.7
```