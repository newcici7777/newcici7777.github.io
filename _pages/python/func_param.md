---
title: 函式作為參數
date: 2026-03-10
keywords: Python, function as parameter
---
函式作為參數傳遞給函式，傳遞過去的是「處理邏輯」。<br>

如果有多個「處理邏輯」，但其它程式碼是固定的，可以把多個處理邏輯抽取出來。<br>

簡言之，就是把「變動」的邏輯抽取出來，「固定」的邏輯放在原處。<br>

以下有三個不同的處理邏輯，分別是一倍獎金、二倍獎金、三倍獎金。<br>
計算薪水的方式是固定，就是獎金 \+ 薪水。<br>
但不同職位有不同獎金的計算方式。<br>

- CEO使用三倍獎金邏輯。
- 主管使用二倍獎金邏輯。
- 職員使用一倍獎金邏輯。

呼叫計算薪水cal_salary函式，並把不同的獎金邏輯代入。<br>
注意！把函式名傳入函式時，圓括號`()`不用傳入，因為不是呼叫函式。<br>

{% highlight python linenos %}
# 獎金邏輯
def bonus_1time(bonus):
    return bonus


def bonus_2times(bonus):
    return bonus * 2


def bonus_3times(bonus):
    return bonus * 3

# 固定計算方式 = 獎金 + 薪水
def cal_salary(param_func, bonus, salary):
    return param_func(bonus) + salary

# 計算薪水
# 代入不同的獎金邏輯
print(f"CEO的bonus = {cal_salary(bonus_3times, 1000, 100000)}")
print(f"主管的bonus = {cal_salary(bonus_2times, 900, 50000)}")
print(f"職員的bonus = {cal_salary(bonus_1time, 800, 20000)}")

{% endhighlight %}
```
CEO的bonus = 103000
主管的bonus = 51800
職員的bonus = 20800
```

把函式名傳入函式的語法:<br>
```
函式(函式名,參數)
cal_salary(bonus_3times, 1000, 100000)
```

作為參數的函式，也可以使用外部參數。<br>
```
def 外部函式(函式名, 外部參數):
   函式名(外部參數)
```

使用type(函式名)就可以輸出類型。<br>
{% highlight python linenos %}
def fun3(param_func, code):
    print(type(param_func))
{% endhighlight %}
```
<class 'function'>
```