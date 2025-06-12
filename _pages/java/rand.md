---
title: 亂數
date: 2025-06-11
keywords: java, rand
---
## Math.random()
產生介於0到1(不包含1)之間的亂數，0 \<= 數字 \< 1，最大是0.999999..
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    // 產生10個亂數
    for (int i = 0; i < 10; i++) {
      System.out.println(Math.random());
    }
  }
}
{% endhighlight %}
```
0.6626947146950831
0.8865498402906333
0.9527716033115488
0.4059555335297955
0.24287815701814686
0.10780772812143591
0.005724639017029354
0.41796994366817686
0.8392257800190969
0.736425852792078
```
## 傳回值是double
random()傳回值是double
{% highlight java linenos %}
public static double random() {
    return RandomNumberGeneratorHolder.randomNumberGenerator.nextDouble();
}
{% endhighlight %}

## 產生0至x亂數
產生0至6的10個亂數。

以下程式碼為何要6 \+ 1 ? 因為亂數產生的是0至 5.999999，產生的範圍是0 \<= x \< 6，不會等於6，所以要加1，加1之後的範圍是0至 6.9999999，最大不會超過7。

因為random傳回值是double，要使用int強制轉型，int強制轉型只會去掉小數點，不會四捨五入。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println((int)5.8);
    for (int i = 0; i < 10; i++) {
      System.out.println((int)(Math.random() * (6 + 1)));
    }
  }
}
{% endhighlight %}
```
3
5
3
5
5
4
1
2
0
4
```

## 產生2至7之間的亂數
原本的Math.random()只會產生0至x的數字。

要產生2至7的亂數，要先產生0至5的亂數，再加上2，才會符合2至7的亂數。
```
7 - 2 = 5
```

但0至5的亂數，只會產生0至4.99999的亂數，所以要再 \+ 1，這樣才會產生0至5.999999的亂數
```
7 - 2 = 5

5 + 1 = 6
```

最後亂數再加2，並且使用強制轉型int，無條件去掉小數點。

公式
```
(int)(Math.random() * (7 - 2 + 1)) + 2
```
{% highlight java linenos %}
for (int i = 0; i < 10; i++) {
  System.out.println((int)(Math.random() * (7 - 2 + 1)) + 2);
}
{% endhighlight %}
```
3
3
3
7
7
2
3
3
2
3
```
