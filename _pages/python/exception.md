---
title: Exception 例外
date: 2026-06-01
keywords: Python, Exception
---
## 例外處理
### 最簡單
語法:
```
try:
    例外
except:
    處理例外
```

{% highlight python linenos %}
try:
    num1 = 10
    num2 = 0
    res = num1 / num2
except:
    print("出現例外")
{% endhighlight %}
```
出現例外
```

### 取得Exception
語法:
```
try:
    例外
except 例外 as 自訂名稱:
    處理例外

try:
    可能有例外的程式碼
except Exception as e:
    取得e例外內容
```

{% highlight python linenos %}
try:
    num1 = 10
    num2 = 0
    res = num1 / num2
except Exception as e:
    print(f"exception: {e} \n"
          "type:", type(e))
{% endhighlight %}
```
exception: division by zero 
type: <class 'ZeroDivisionError'>
```

### 多個except 與 finally
語法:
```
try:
    例外
except 例外 as 自訂名稱:
    處理例外
else:(可省略不寫)
    沒有例外發生
finally:(可省略不寫)
    不管有沒有例外都執行    
```
注意！以下程式碼，執行到`res = num1 / num2`就產生例外，之後的`open("/目錄/檔案","r")`就不會執行。<br>
{% highlight python linenos %}
try:
    num1 = 10
    num2 = 0
    res = num1 / num2
    open("/目錄/檔案","r")
except ZeroDivisionError as e:
    print(f"ZeroDivisionError\n")
except Exception as e:
    print(f"exception: {e} \n")
finally:
    print(f"finally: \n")
{% endhighlight %}
```
ZeroDivisionError

finally:
```

### 取得多個except
{% highlight python linenos %}
try:
    num1 = 10
    num2 = 0
    res = num1 / num2
    open("/目錄/檔案","r")
except (ZeroDivisionError,FileNotFoundError) as e:
    print(e)
finally:
    print(f"finally: \n")
{% endhighlight %}
```
division by zero
finally:
```

### raise
{% highlight python linenos %}
try:
    raise ZeroDivisionError("主動拋出zero例外")
except (ZeroDivisionError,FileNotFoundError) as e:
    print(f"exception: {e} type: {type(e)}")
{% endhighlight %}
```
exception: 主動拋出zero例外 type: <class 'ZeroDivisionError'>
```

### 如果沒人處理exception 由源頭處理
下方程式碼，f3() 沒有處理exception，返回f2()，但f2()也沒處理exception，返回f1()，由f1()處理。<br>
{% highlight python linenos %}
def f1():
    print("-----f1 start ----")
    try:
        f2()
    except :
        print("exception")
    print("-----f1 end ----")
def f2():
    print("-----f2 start ----")
    f3()
    print("-----f2 end ----")
def f3():
    print("-----f3 start ----")
    print(10 / 0)
    print("-----f3 end ----")

f1()
{% endhighlight %}
```
-----f1 start ----
-----f2 start ----
-----f3 start ----
exception
-----f1 end ----
```
## Error 種類
### IndexError
{% highlight python linenos %}
try:
    str = "Hello"
    print(str[100])
except Exception as e:
    print(e)
{% endhighlight %}
```
string index out of range
```

### KeyError
{% highlight python linenos %}
    dict1 = {"name":"cici", "age":18}
    print(dict1["address"])
{% endhighlight %}

### TypeError
{% highlight python linenos %}
try:
    a = "Hello"
    b = 5
    print(a + b)
except Exception as e:
    print(e)
{% endhighlight %}
```
can only concatenate str (not "int") to str
```

### ValueError
{% highlight python linenos %}
try:
    print(int("Hello"))
except Exception as e:
    print(e)
{% endhighlight %}
```
invalid literal for int() with base 10: 'Hello'
```

### FileNotFoundError
{% highlight python linenos %}
# /Users/cici/testc/file_test2
try:
    f = open("/目錄/檔名", "r", encoding="utf-8")
except Exception as e:
    print(e)
{% endhighlight %}
```
[Errno 2] No such file or directory: '/目錄/檔名'
```

### AttributeError
沒有這個屬性
{% highlight python linenos %}
class A:
    def hi(self):
        pass
a = A()
print(a.name)
{% endhighlight %}

