---
title: ?與extends與super
date: 2025-05-28
keywords: Java, Generics ? Wildcard, Generics extends, Generics super
---
## 泛型類型沒有多型

- [多型][1]

不能像類別多型一樣，等號=左邊是父類別，等號=右邊是指向子類別。

以下的語法，不能編譯通過，即便String是Object的子類別。
{% highlight java linenos %}
List<Object> list = new ArrayList<String>();
{% endhighlight %}

## ? extends
泛型類型只能是Number或Number的子類別。
{% highlight java linenos %}
< ? extends Number>
{% endhighlight %}

下圖中，只有Boolean與Character沒有在Number下。

![img]({{site.imgurl}}/java/basic_type_extends.png)

範例程式碼
{% highlight java linenos %}
public class Test1 {
  public static void main(String[] args) {
    ArrayList<Integer> list = new ArrayList<>();
    list.add(5);
    list.add(4);
    printNumberList(list);
    
    ArrayList<Character> charlist = new ArrayList<>();
    // 以下編譯不過，因為Character不是Number子類
    printNumberList(charlist);
  }

  // 參數泛型類型只能是Number或Number的子類別
  public static void printNumberList(List<? extends Number> list) {
    for (Number number : list) {
      System.out.println(number);
    }
  }
{% endhighlight %}
```
5
4
```

## ? supper
泛型類型只能是Integer或Integer父類別。
{% highlight java linenos %}
< ? supper Integer>
{% endhighlight %}

範例程式碼
{% highlight java linenos %}
public class Test1 {
  public static void main(String[] args) {
    ArrayList<Integer> list = new ArrayList<>();
    list.add(5);
    list.add(4);
    // 以下編譯不過，因為Integer不是Character，也不是Character父類別。
    printParent(list);

    ArrayList<Character> charlist = new ArrayList<>();
    printParent(charlist);
    ArrayList<Object> objlist = new ArrayList<>();
    printParent(objlist);
  }

  // 參數泛型類型只能是Number或Number的子類別
  public static void printNumberList(List<? extends Number> list) {
    for (Number number : list) {
      System.out.println(number);
    }
  }

  // 參數泛型類型只能是Character或Character父類別
  public static void printParent(List<? super Character> list) {
    for (Object o : list) {
      System.out.println(o);
    }
  }
}
{% endhighlight %}

## ? 任何類型
?代表泛型類型可以是任何類型。
{% highlight java linenos %}
< ? >
{% endhighlight %}

不管任何類型的list都可以使用printAnyType()方法。
{% highlight java linenos %}
public class Test1 {
  public static void main(String[] args) {
    ArrayList<Integer> list = new ArrayList<>();
    list.add(5);
    list.add(4);
    printAnyType(list);
    
    ArrayList<Character> charlist = new ArrayList<>();
    printAnyType(charlist);
    
    ArrayList<Object> objlist = new ArrayList<>();
    printAnyType(objlist);
  }

  // 參數泛型類型只能是Number或Number的子類別
  public static void printNumberList(List<? extends Number> list) {
    for (Number number : list) {
      System.out.println(number);
    }
  }

  // 參數泛型類型只能是Character或Character父類別
  public static void printParent(List<? super Character> list) {
    for (Object o : list) {
      System.out.println(o);
    }
  }

  // 參數泛型類型可以是任何類型
  public static void printAnyType(List<?> list) {
    for (Object o : list) {
      System.out.println(o);
    }
  }
}
{% endhighlight %}

[1]: {% link _pages/java/polymorphism.md %}