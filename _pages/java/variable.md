---
title: 函式可變參數
date: 2025-09-15
keywords: java, variable parameter 
---
## 函式可變參數語法
參數類型後面加上3個點。<br>
```
回傳值類型 函式名(類型... 變數名)
void func1(int... nums)
```

## 可變參數是陣列
可變參數的本質是陣列，是傳遞一個陣列給函式。<br>

以下程式碼，輸出nums陣列大小。
{% highlight java linenos %}
public class Variable {
  public static void main(String[] args) {
    Variable variable = new Variable();
    variable.func1(10, 20, 30);
  }
  public void func1(int... nums) {
    System.out.println(nums.length);
  }
}
{% endhighlight %}
```
3
```

用for輸出每個元素。
{% highlight java linenos %}
public class Variable {
  public static void main(String[] args) {
    Variable variable = new Variable();
    variable.func1(10, 20, 30);
  }
  public void func1(int... nums) {
    for (int i = 0; i < nums.length; i++) {
      System.out.println(nums[i]);
    }
  }
}
{% endhighlight %}
```
10
20
30
```

## 函式多參數
函式有多個參數，可變參數一定要放在最後面。
{% highlight java linenos %}
public class Variable {
  public static void main(String[] args) {
    Variable variable = new Variable();
    variable.func1('a', 55.0,10, 20, 30);
  }
  public void func1(char c, double d,int... nums) {
    System.out.println("c = " + c);
    System.out.println("d = " + d);
    for (int i = 0; i < nums.length; i++) {
      System.out.println(nums[i]);
    }
  }
}
{% endhighlight %}
```
c = a
d = 55.0
10
20
30
```

## 可以不傳參數
可變參數可以不傳參數
{% highlight java linenos %}
public class Variable {
  public static void main(String[] args) {
    Variable variable = new Variable();
    variable.func1();
  }
  public void func1(int... nums) {
    System.out.println(nums.length);
  }
}
{% endhighlight %}
```
0
```

## 函式只能有一個可變參數
以下參數有二個可變參數，編譯失敗。
{% highlight java linenos %}
  public void func1(int... nums, String... args) {
    System.out.println(nums.length);
  }
{% endhighlight %}
