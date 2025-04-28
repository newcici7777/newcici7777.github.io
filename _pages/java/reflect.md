---
title: 反射
date: 2025-04-28
keywords: Java, reflect
---
反射英文是Reflection

## Class
Class知道類別的屬性、方法，透過Class類別可以取得屬性、方法、建構子。

## 測試的程式碼
以下程式碼有靜態區塊static{...}與建構子
{% highlight java linenos %}
public class Duck extends Animal implements Fly, Swim{
  public String name;
  private String info;

  static {
    System.out.println("Duck 被classloader載入");
  }

  public Duck() {
    System.out.println("物件被jvm建立");
  }

  private Duck(String name) {
    this.name = name;
  }

  @Override
  public void eat() {
    System.out.println("鴨子吃");
  }

  @Override
  public void fly() {
    System.out.println("鴨子飛");
  }

  @Override
  public void swim() {
    System.out.println("鴨子游泳");
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public String getInfo() {
    return info;
  }

  public void setInfo(String info) {
    this.info = info;
  }
}
{% endhighlight %}

## Class.forName
forName("")的參數是package名字.類別名組成，用點來區隔。

執行結果會被classLoader載入
{% highlight java linenos %}
Class class1 = Class.forName("reflect.Duck");
System.out.println(class1);
{% endhighlight %}
```
Duck 被classloader載入
class reflect.Duck
```

## 類別名.class
執行結果「不會」被classLoader載入
{% highlight java linenos %}
Class class2 = Duck.class;
System.out.println(class2);
{% endhighlight %}
```
class reflect.Duck
```

## getClass()
執行結果會被classLoader載入
{% highlight java linenos %}
Duck duck = new Duck();
Class class3 = duck.getClass();
System.out.println(class3);
{% endhighlight %}
```
Duck 被classloader載入
物件被jvm建立
class reflect.Duck
```

## 取得父類別
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws Exception {
    Class class1 = Duck.class;
    Class superclass = class1.getSuperclass();
    System.out.println(superclass);
  }
}
{% endhighlight %}
```
class reflect.Animal
```

## 取得介面
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws Exception {
    Class class1 = Duck.class;
    Class[] interfaces = class1.getInterfaces();
    for (Class cls : interfaces) {
      System.out.println(cls);
    }
  }
}
```
interface reflect.Fly
interface reflect.Swim
```
{% endhighlight %}

## 取得public屬性
{% highlight java linenos %}
  Class class1 = Duck.class;
  Field[] fields = class1.getFields();
  for (Field field : fields) {
    System.out.println(field);
  }
{% endhighlight %}
```
public java.lang.String reflect.Duck.name
```

## 取得所有屬性
{% highlight java linenos %}
  Field[] dfields = class1.getDeclaredFields();
  for (Field field : dfields) {
    System.out.println(field);
  }
{% endhighlight %}
```
public java.lang.String reflect.Duck.name
private java.lang.String reflect.Duck.info
```

## 取得public方法與所有方法
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws Exception {
    Class class1 = Duck.class;
    Method[] methods = class1.getMethods();

    Method[] dmethods = class1.getDeclaredMethods();
    for (Method method : dmethods) {
      System.out.println(method);
    }
  }
}
{% endhighlight %}
```
public java.lang.String reflect.Duck.getInfo()
public void reflect.Duck.eat()
public void reflect.Duck.fly()
public void reflect.Duck.swim()
public void reflect.Duck.setInfo(java.lang.String)
```

## 取得public建構子與private建構子
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws Exception {
    Class class1 = Duck.class;
    Constructor[] constructors = class1.getConstructors();

    Constructor[] dconstructors = class1.getDeclaredConstructors();
    for (Constructor constructor : dconstructors) {
      System.out.println(constructor);
    }
  }
}
{% endhighlight %}
```
public reflect.Duck()
private reflect.Duck(java.lang.String)
```

## 使用private建構子建立物件並呼叫私有方法
先把public建構子
{% highlight java linenos %}
  public Duck() {
    System.out.println("物件被jvm建立");
  }
{% endhighlight %}

改成
{% highlight java linenos %}
  private Duck() {
    System.out.println("物件被jvm建立");
  }
{% endhighlight %}

增加private 方法
{% highlight java linenos %}
  private void getAddress() {
    System.out.println("private getAddress() test");
  }
{% endhighlight %}

呼叫private建構子與private方法
{% highlight java linenos %}
public static void main(String[] args) throws Exception {
  Class class1 = Duck.class;
  // 呼叫private建構子建立物件
  Constructor dc = class1.getDeclaredConstructor();
  // 把private改成public
  dc.setAccessible(true);
  Object instance = dc.newInstance();
  // 呼叫私有方法
  Method dm1 = class1.getDeclaredMethod("getAddress");
  // 設置成public
  dm1.setAccessible(true);
  // 參數代入instance
  dm1.invoke(instance);
}
{% endhighlight %}
```
Duck 被classloader載入
物件被jvm建立
private getAddress() test
```