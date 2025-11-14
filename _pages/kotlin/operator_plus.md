---
title: plus運算子
date: 2025-06-09
keywords: kotlin, plus operator
---
## operator

讓二個類別可以進行加減乘除。<br>
使用像 +、-、\* 這些語法。<br>
```
A類別 + B類別
A類別 - B類別
A類別 * B類別
```

|運算子|	覆寫方法|	說明|
|:---|:-----|:-----|
|+	|plus	|加法|
|-	|minus	|減法|
|*	|times	|乘法|
|/	|div	|除法|
|%	|rem	|取餘數|

```
新的員工 = emp1員工的薪水 + emp2員工的薪水
new_emp = emp1 + emp2
```

把員工相加 \+ 運算子的方法覆寫，並傳回Emp物件。<br>
{% highlight kotlin linenos %}
class Emp (val empId: String, val salary: Int){
    operator fun plus(other: Emp): Emp {
        return Emp("", other.salary + this.salary)
    }
}
{% endhighlight %}

2個員工薪水相加。
{% highlight kotlin linenos %}
fun main() {
    val emp1 = Emp("001" , 50000)
    val emp2 = Emp("002", 30000)
    val new_emp = emp1 + emp2
    println(new_emp.salary)
}
{% endhighlight %}
```
80000
```

座標二點 x、y 位置相加。<br>
{% highlight kotlin linenos %}
class Point(val x: Int, val y: Int) {
    operator fun plus(other: Point) :Point {
        return Point(x + other.x, y + other.y)
    }
}

fun main() {
    val point1 = Point(1 , 3)
    val point2 = Point(2, 5)
    val new_point = point1 + point2
    println("new point x = ${new_point.x} y = ${new_point.y}")
}
{% endhighlight %}
```
new point x = 3 y = 8
```