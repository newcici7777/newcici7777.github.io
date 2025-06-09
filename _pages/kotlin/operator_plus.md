---
title: plus運算子
date: 2025-06-09
keywords: kotlin, plus operator
---
把\+運算子的方法覆寫。
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
    val sum = emp1 + emp2
    println(sum.salary)
}
{% endhighlight %}
```
80000
```