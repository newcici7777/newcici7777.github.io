---
title: private public
date: 2026-03-24
keywords: Python, private public
---
預設成員變數、成員方法都是public，除非在成員變數與方法前加上`__`二個底線，才會變成private。<br>

## 語法
`__`二個底線宣告屬性或方法是私有。<br>
私有屬性、私有方法，只供類別內部讀取與寫入，若要提供外部使用，需提供公有public方法供外部呼叫。<br>

```
class 類別:
	__私有屬性 = None

	def __私有方法(self):
		程式碼
```

## private 屬性
以下宣告`__salary`私有屬性，並提供get_salary()與set_salary()公有方法供外部使用。<br>

{% highlight python linenos %}
class Employee:
    name = None
    __salary = None

    def __init__(self, name, salary):
        self.name = name
        self.__salary = salary

    def get_salary(self):
        return self.__salary

    def set_salary(self, salary):
        self.__salary = salary


emp1 = Employee("Mary", 20000)
print(f"name = {emp1.name} salary = {emp1.get_salary()}")
emp1.set_salary(30000)
print(f"name = {emp1.name} salary = {emp1.get_salary()}")
{% endhighlight %}
```
name = Mary salary = 20000
name = Mary salary = 30000
```
## private 方法
私有方法只供內部類別呼叫。<br>
{% highlight python linenos %}
class Employee:
    name = None
    level = None
    __salary = None

    def __init__(self, name, level, salary):
        self.name = name
        self.level = level
        self.__salary = salary

    def get_salary(self):
        return self.__salary + self.__bonus()

    def set_salary(self, salary):
        self.__salary = salary

    def __bonus(self):
        if self.level == 1:
            return self.__salary * 0.1
        elif self.level == 2:
            return self.__salary * 0.2
        else:
            return self.__salary * 0.3

emp1 = Employee("Mary", 1, 20000)
print(f"emp1 name = {emp1.name} salary = {emp1.get_salary()}")
emp2 = Employee("Bill", 2, 20000)
print(f"emp2 name = {emp2.name} salary = {emp2.get_salary()}")
{% endhighlight %}
```
emp1 name = Mary salary = 22000.0
emp2 name = Bill salary = 24000.0
```

## 類別外私有屬性不可指派值
在類別外，私有屬性設值是沒用。
```
物件.__私有屬性 = 值
```

以下程式碼，在類別外，設定__salary為50000，但實際輸出仍是20000
{% highlight python linenos %}
class Employee:
    name = None
    __salary = None

    def __init__(self, name, salary):
        self.name = name
        self.__salary = salary

    def get_salary(self):
        return self.__salary

    def set_salary(self, salary):
        self.__salary = salary

emp1 = Employee("Mary", 20000)
# 以下私有屬性設成50000，無作用
emp1.__salary = "500000"
print(emp1.get_salary())
{% endhighlight %}
```
20000
```