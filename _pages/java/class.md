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

下圖請見藍色字。<br>

![img]({{site.imgurl}}/java/class_obj2.png)<br>

1. 編譯階段取得Class物件 Class.forName()
2. 由類別名，取得Class物件 類別名.class
3. 由物件取得Class物件，object.getClass()
4. 由ClassLoader取得Class物件，ClassLoader.loadClass()

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

執行結果，左邊顯示的是class，右邊顯示的是指向什麼類別的結構，但它不是Cat的物件，它是Class的物件，指向Cat類別的結構。<br>
```
class reflect.Cat
class reflect.Cat
class reflect.Cat
class reflect.Cat
```

## Heap中，只有一份 Class 物件
Heap中，僅存一份的 Class 物件，Class物件包含類別所有結構(成員變數、成員方法、變數方法存取權限、父類別)，透過Class 物件，可以建立cat1物件、cat2物件、cat3物件。

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
使用Class的靜態方法forName()，參數代入`package + 類別名`<br>
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

以下是一個 Java 寫的透過Class物件，傳回某類型的物件，回傳類型取決於泛型類型T是什麼，createObj() 參數傳入`Class 物件`。<br>
透過Class的newInstance()，建立物件，並把物件傳回，注意！這邊傳回物件，不是Class物件，是泛型類型T物件。<br>
```
public 傳回類型 T createObj(Class<類型> clazz) {
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
class的回傳值是包裝類別。

{% highlight java linenos %}
Class clazz = Integer.class;
System.out.println(clazz);
{% endhighlight %}
```
class java.lang.Integer
```

語法
```
Class clazz = 包裝類別.TYPE;
```

TYPE的回傳值是把包裝類別，自動拆箱成基本型別。<br>
{% highlight java linenos %}
Class wrap_clz = Integer.TYPE;
System.out.println("wrap_clz=" + wrap_clz);
{% endhighlight %}
```
wrap_clz=int
```
## 以下類型都有Class物件
- 一般類別
- 內部類別,匿名內部類別,靜態內部類別
- interface
- 陣列
- enum
- annotation
- 基本類型
- 包裝類別
- void 傳回值類型為空
- Class

{% highlight java linenos %}
public class ReflectTest {
  public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
    Class dog_clz = Dog.class;
    System.out.println("dog_clz = " + dog_clz);
    Class interface_clz = Fly.class;
    System.out.println("interface_clz = " + interface_clz);
    Class enum_clz = Gender1.class;
    System.out.println("enum_clz = " + enum_clz);
    Class arr_clz = int[].class;
    System.out.println("arr_clz = " + arr_clz);
    Class arr2d_clz = int[][].class;
    System.out.println("arr2d_clz = " + arr2d_clz);
    Class anotation_clz = Deprecated.class;
    System.out.println("anotation_clz = " + anotation_clz);
    Class void_clz = void.class;
    System.out.println("void_clz = " + void_clz);
    // Class本身也有Class物件
    Class class_clz = Class.class;
    System.out.println("class_clz = " + class_clz);
    Class wrap_clz = Integer.class;
    System.out.println("wrap_clz=" + wrap_clz);
  }
}

class Dog {}

interface Fly {
  void fly();
}

enum Gender1 {
  BOY, GIRL;
}
{% endhighlight %}
```
dog_clz = class reflect.Dog
interface_clz = interface reflect.Fly
enum_clz = class reflect.Gender1
arr_clz = class [I
arr2d_clz = class [[I
anotation_clz = interface java.lang.Deprecated
void_clz = void
class_clz = class java.lang.Class
wrap_clz=class java.lang.Integer
```

## Class物件用途
### 建立物件
除了`new`的方式建立物件之外，也可以使用Class物件的newInstance()方法，但要先取得Class物件。<br>
語法<br>
```
Class clz = Cat.class;
Object obj = clz.newInstance();
System.out.println("obj = " + obj);
```

但輸出obj時會顯示真正的執行類型為Cat，並非Object。
```
obj = reflect.Cat@3af49f1c
```

newInstance()傳回值是Object，要向下強制轉型成Cat類別。
```
Cat cat = (Cat)clz.newInstance();
```

### 錯誤建立物件與轉型
Class物件，不是Cat類型，以下程式碼是錯誤的！
{% highlight java linenos %}
Class clz = Cat.class;
Cat cat1 = (Cat) clz;
{% endhighlight %}
```
Inconvertible types; cannot cast 'java.lang.Class' to 'reflect.Cat'
```
編譯錯誤的原因是，Class類別不能轉成Cat類別。

### Class物件都是相同hash code
Cat class:<br>
{% highlight java linenos %}
public class Cat {
  public String name = "Mary";
  public void method1() {}
}
{% endhighlight %}

不管是clz的hash code，`obj.getClass()`，`cat.getClass()`，三者取得的Class物件都是同一個，都是868693306。
{% highlight java linenos %}
public class ReflectTest2 {
  public static void main(String[] args) throws InstantiationException, IllegalAccessException {
    // 取得Class
    Class clz = Cat.class;
    Object obj = clz.newInstance();
    System.out.println("obj = " + obj);
    Cat cat = (Cat)clz.newInstance();
    System.out.println("cat = " + cat);

    System.out.println("clz hashcode = " + clz.hashCode());
    System.out.println("obj class hashcode = " + obj.getClass().hashCode());
    System.out.println("cat hashcode = " + cat.getClass().hashCode());
  }
}
{% endhighlight %}
```
obj = reflect.Cat@3af49f1c
cat = reflect.Cat@19469ea2
clz hashcode = 868693306
obj class hashcode = 868693306
cat hashcode = 868693306
```

## 靜態加載 動態加載
### 靜態加載
沒有執行到類別，也會把它的Class物件，編譯階段，加載到Heap記憶體中。<br>

以下程式碼，編譯到下面這段，就會把Dog的Class物件加載到Heap記憶體中。<br>
只是編譯，尚未執行就載入到記憶體。<br>
```
Dog dog = new Dog();
```

### 動態加載
使用`Class.forName()`，即便編譯到這一行，也不會把abcd加載進來，不會編譯錯誤，當使用者輸入「2」，才會加載abcd，才會發現沒這個類別。<br>

但編譯是通過的，執行到`Class.forName()`，才會試圖加載Class物件。<br>

### 完整程式碼
{% highlight java linenos %}
public class DynamicTest {
  public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
    Scanner scanner = new Scanner(System.in);
    System.out.println("Please input number:");
    String key = scanner.next();
    switch (key) {
      case "1":
        Dog dog = new Dog();
        System.out.println(dog.toString());
        break;
      case "2":
        Class cls = Class.forName("abcd");
        Object o = cls.newInstance();
        Method m = cls.getMethod("method1");
        m.invoke(o);
        System.out.println("ok");
        break;
      default:
        System.out.println("do nothing");
    }
  }
}
{% endhighlight %}
```
Please input number:
2
Exception in thread "main" java.lang.ClassNotFoundException: abcd
  at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641)
  at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188)
  at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
  at java.base/java.lang.Class.forName0(Native Method)
  at java.base/java.lang.Class.forName(Class.java:391)
  at java.base/java.lang.Class.forName(Class.java:382)
  at reflect.DynamicTest.main(DynamicTest.java:18)
```

### 加載分類
靜態加載:編譯時就加載到記憶體。<br>
1. 使用new建立物件。
2. 子類被加載，父類也會被加載。
3. 呼叫到靜態屬性、靜態方法，類別會被加載。<br>

動態加載: 執行到才加載。<br>

1. Class.forName()

