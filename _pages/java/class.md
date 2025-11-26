---
title: Class
date: 2025-11-26
keywords: Java, Class
---
## Class 類別
Class 類別繼承圖。<br>
![img]({{site.imgurl}}/java/class_extends.png)<br>

Class 是一個類別，繼承 Object類別，實作 Serializable介面，實作 AnnotateElement 介面，AnnotateElement 介面繼承AccessibleObject類別。<br>

以下介紹這些類別的功能:<br>
- Class 包含類別所有結構(成員變數、成員方法、變數方法存取權限、父類別)。
- Object 「Class」的父類別，所有類別的父類別，getClass()可以取得物件的Class「物件」。
- AccessibleObject 修改成員變數、方法的存取權限，如private修改成public。
- Serializable 可以把物件支援IO功能，讀取寫入物件。

## Class物件 
產生Class物件。<br>
![img]({{site.imgurl}}/java/class_obj1.png)<br>

1. 程式碼，透過javac，產生.class的byteCode檔案。
2. 程式碼中第一次new Cat()，呼叫ClassLoader
3. ClassLoader在Heap的記憶體位置建立Class物件。
4. ClassLoader在Meta space的位置建立Meta data。

只有第1次new，才會啟動ClassLoader，因為Heap記憶體中，只會有一個Class 物件。

下圖中，Cat物件與Cat的Class物件，存放在Heap中。<br>
而Meta Space中存放Cat的Meta data。<br>
![img]({{site.imgurl}}/java/class1.png)<br>

## 取得Class物件
4種方法，可以取得Class物件。<br>

![img]({{site.imgurl}}/java/class_obj2.png)<br>

1. 編譯階段取得Class Class.forName()
2. 由類別名，取得Class 類別名.class
3. 由物件取得Class，cat物件.getClass()
4. 由ClassLoader取得Class，ClassLoader.loadClass()

{% highlight java linenos %}
public class ReflectTest {
  public static void main(String[] args) throws ClassNotFoundException {
    // 1.編譯階段 參數必須為package.類別名
    Class catClz1 = Class.forName("reflect.Cat");
    System.out.println(catClz1);
    // 2.透過類別名.class
    Class catClz2 = Cat.class;
    System.out.println(catClz2);
    // 3.透過物件Object.getClass()
    Cat cat = new Cat();
    Class catClz3 = cat.getClass();
    System.out.println(catClz3);
    // 4.透過ClassLoader
    ClassLoader loader = cat.getClass().getClassLoader();
    Class catClz4 = loader.loadClass("reflect.Cat");
    System.out.println(catClz4);
  }
}
{% endhighlight %}
```
class reflect.Cat
class reflect.Cat
class reflect.Cat
class reflect.Cat
```

## Heap中，只有一份 Class 物件
透過Heap中，僅存一份的 Class 物件，Class物件包含類別所有結構(成員變數、成員方法、變數方法存取權限、父類別)，透過Class 物件，可以建立cat1物件、cat2物件、cat3物件。

![img]({{site.imgurl}}/java/class_obj3.png)

而cat1、cat2、cat3透過Object.getClass()都能取得class物件，取得的class物件是同一個，因為class物件只會存在Heap中一份。<br>

以下程式碼印出的hashCode都是868693306，代表各種不同方式取得的Class物件，指向的都是同一個Class物件。
{% highlight java linenos %}
public class ReflectTest {
  public static void main(String[] args) throws ClassNotFoundException {
    // 1.
    Class catClz1 = Class.forName("reflect.Cat");
    System.out.println(catClz1);
    // 2.
    Class catClz2 = Cat.class;
    System.out.println(catClz2);
    // 3.
    Cat cat = new Cat();
    Class catClz3 = cat.getClass();
    System.out.println(catClz3);
    // 4.
    ClassLoader loader = cat.getClass().getClassLoader();
    Class catClz4 = loader.loadClass("reflect.Cat");
    System.out.println(catClz4);

    System.out.println(catClz1.hashCode());
    System.out.println(catClz2.hashCode());
    System.out.println(catClz3.hashCode());
    System.out.println(catClz4.hashCode());
  }
}
{% endhighlight %}
```
868693306
868693306
868693306
868693306
```

## 使用場景
有四種取得Class 物件的方式，也代表4種不同的場景的使用方式。

### 編譯階段
`Class.forName(package + 類別名)`，取得Class物件。<br>
{% highlight java linenos %}
    Class catClz1 = Class.forName("reflect.Cat");
    System.out.println(catClz1);
{% endhighlight %}

### 參數傳遞
透過泛型傳入Class，再透過Class物件，建立某種類型的物件。<br>

語法<br>
```
Class<類型>
Class<T>
```

Retrofit 是一個 Java 寫的函式庫，它的 create() 參數傳入`Class 物件`。<br>
透過Class的newInstance()，建立物件，並把物件傳回，注意！這邊傳回物件，不是Class物件，是Cat物件。<br>
```
public 傳回類型 T create(Class<類型> clazz) {
  return clazz.newInstance()
}
public <T> T createObj(Class<T> clazz) {
  return clazz.newInstance()
}
public Cat createObj(Class<Cat> clazz) {
  return clazz.newInstance()
}
```

使用方法:
```
Cat cat = retrofit.createObj(Cat.class)
```

完整程式碼:
{% highlight java linenos %}
public class ReflectTest {
  public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
    ReflectTest test = new ReflectTest();
    // 傳入Class物件，透過Class物件，建立Cat物件，並傳回Cat物件。
    Cat cat1 = test.createObj(Cat.class);
    System.out.println(cat1);
  }
  public <T> T createObj(Class<T> clazz) throws InstantiationException, IllegalAccessException {
    return clazz.newInstance();
  }
}
{% endhighlight %}
```
reflect.Cat@3af49f1c
```

### 通過Cat物件，取得Class物件
語法
```
物件Object.getClass();
Cat cat = new Cat();
Class clazz = cat.getClass();
```

## 基本類型的Class物件
語法
```
Class clazz = 基本型別.class;
Class clazz = int.class;
Class clazz = char.class;
Class clazz = boolean.class;
```

{% highlight java linenos %}
Class clazz = int.class;
System.out.println(clazz);
{% endhighlight %}
```
int
```

## 包裝類型的Class物件
語法
```
Class clazz = 包裝類別.class;
Class clazz = Integer.class;
```

{% highlight java linenos %}
Class clazz = Integer.class;
System.out.println(clazz);
{% endhighlight %}
```
class java.lang.Integer
```

## 以下類型都有Class物件
- 外部類別,內部類別,匿名內部類別,靜態內部類別
- interface
- 陣列
- enum
- annotation
- 基本類型
- 包裝類別
- void