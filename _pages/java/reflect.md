---
title: 反射
date: 2025-04-28
keywords: Java, reflect
---
Prerequisites:

- [Memory Model][1]
- [metadata][2]
- [Classloader類別載入][3]

![img]({{site.imgurl}}/java/class1.png)

在[Classloader][3]文章已經說明Class類別載入的過程。

反射英文是Reflection

## Class
Class知道類別的所有屬性、方法，透過Class類別可以取得屬性、方法、建構子。

## 測試的程式碼
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
{% endhighlight %}
```
interface reflect.Fly
interface reflect.Swim
```

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
    // getMethods只有public的方法
    Method[] methods = class1.getMethods();
    for (Method method : methods) {
      System.out.println(method);
    }

    // getDeclaredMethods() 包含所有public,private方法
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

## 取得父類別所有方法
{% highlight java linenos %}
public class Test3 {
  public static void main(String[] args) {
    Class<?> clazz = Dog.class;
    System.out.println("Methods in class: " + clazz.getName());
    for (Method method : clazz.getDeclaredMethods()) {
      System.out.println(method);
    }

    // parent
    Class<?> superclass = clazz.getSuperclass();
    System.out.println("Methods in Parent: " + clazz.getName());
    for (Method method : superclass.getDeclaredMethods()) {
      System.out.println(method);
    }
  }
}

class Animal {
  int i = 10;  // 父類欄位
  void speak() { System.out.println(i); }  // 印出 this.i
}

class Dog extends Animal {
  int i = 5;  // 子類同名欄位
  //void speak() { System.out.println(i); }
}
{% endhighlight %}
```
Methods in class: wrap.Dog
Methods in Parent: wrap.Dog
void wrap.Animal.speak()
```

將以上取得父類別方法改善如下，方便以後做測試，只要輸入「package.類別名」。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws ClassNotFoundException {
    // 參數是「package.類別名」
    getClzMethod(Class.forName("inherit.A"));
  }

  public static void getClzMethod(Class<?> clz) {
    Class<?> clazz = clz;
    System.out.println("Methods in class: " + clazz.getName());
    for (Method method : clazz.getDeclaredMethods()) {
      System.out.println(method);
    }
    System.out.println("------------------");
    // parent
    Class<?> superclass = clazz.getSuperclass();
    System.out.println("Methods in Parent: " + clazz.getName());
    for (Method method : superclass.getDeclaredMethods()) {
      System.out.println(method);
    }
  }
}
{% endhighlight %}

## 取得public建構子與private建構子
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) throws Exception {
    Class class1 = Duck.class;
    Constructor[] constructors = class1.getConstructors();

    // getDeclaredConstructors() 包含所有public,private
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

## 使用public建構子與參數
{% highlight java linenos %}
import java.lang.reflect.Method;
import java.util.Scanner;
public class Test {
  public static void main(String[] args) throws Exception {
    Scanner  sc= new Scanner(System.in);
    System.out.println("輸入package.類別");
    String strName = sc.next();
    System.out.println("請輸入一個數");
    int a = sc.nextInt();
    System.out.println("請輸入另一個數");
    int b = sc.nextInt();
    Class class1 = Class.forName(strName);
    // 呼叫public建構子，建立物件
    Object ins = class1.newInstance();
    // 取出方法與設定參數類型
    Method md = class1.getDeclaredMethod("sum", int.class, int.class);
    // 呼叫方法，第1個參數是物件，a與b是方法參數
    md.invoke(ins, a, b);
  }
}
{% endhighlight %}

編譯，並執行

## 在另一個Project，跟上一個程式碼同一個package下寫以下程式碼，並編譯
{% highlight java linenos %}
public class Cal {
  public void sum(int a, int b) {
    System.out.println("result = ");
    System.out.println(a + b);
  }
}
{% endhighlight %}

## 熱部署
熱部署（英：Hot deployment）是，伺服器不需要重新啟動的情況下，修改軟體或者軟體。
將上面的程式碼產生出來的class檔放在已經正在運行的class目錄下

回到剛才執行"使用public建構子與參數"，輸入package name與類別
```
輸入package.類別
reflect.Cal
請輸入一個數
5
請輸入另一個數
3
result = 
8
```

[1]: {% link _pages/java/memory_model.md %}
[2]: {% link _pages/java/metadata.md %}
[3]: {% link _pages/java/classloader.md %}