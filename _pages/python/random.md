---
title: random randint() randrange()
date: 2026-03-03
keywords: Python, random, randint, randrange
---
## import
使用前要import random
```
import random
```
## 產生隨機數字的範圍
a <= n <= b， 包含b
```
random.randint(a, b)
```

a <= n < b ,不包含b
```
random.randrange(a, b)
```

## randint
{% highlight python linenos %}
import random

n = random.randint(1, 3)
print(n)
n = random.randint(1, 3)
print(n)
n = random.randint(1, 3)
print(n)
{% endhighlight %}
```
2
2
3
```

## randrange
不管執行幾次，都不會有3。<br>
{% highlight python linenos %}
import random

n = random.randrange(1, 3)
print(n)
n = random.randrange(1, 3)
print(n)
n = random.randrange(1, 3)
print(n)
{% endhighlight %}
```
1
2
2
```

## choice
隨機傳回其中的元素。<br>
{% highlight python linenos %}
print(random.choice(['Hello', 'Marry', 'Happy']))
{% endhighlight %}
```
Happy
```
