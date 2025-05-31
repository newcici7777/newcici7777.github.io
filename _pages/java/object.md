---
title: Object
date: 2025-05-31
keywords: Java, Object, toString, hashcode
---
## toString()
印出package\+class name@16進制的hashCode
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Test test = new Test();
    System.out.println(test.toString());
  }
}
{% endhighlight %}
```
inherit2.Test@33c7353a
```

印出物件，也是呼叫toString()的方法，以下程式碼的結果跟上面的一樣。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Test test = new Test();
    System.out.println(test);
  }
}
{% endhighlight %}
```
inherit2.Test@33c7353a
```

## hashCode()
使用記憶體位址進行運算，每個物件的hashCode是不相同的。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Test test = new Test();
    System.out.println(test.hashCode());
  }
}
{% endhighlight %}
```
868693306
```