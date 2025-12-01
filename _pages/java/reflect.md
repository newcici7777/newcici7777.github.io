---
title: 反射
date: 2025-04-28
keywords: Java, reflect
---
Prerequisites:

- [Class][4]
- [Memory Model][1]
- [metadata][2]
- [Classloader類別載入][3]

反射有包含以下四種。

- java.lang.Class 代表類別物件
- java.lang.reflect.Method 成員方法
- java.lang.reflect.Constructor 建構子
- java.lang.reflect.Field 成員變數(屬性)

## 使用反射建立物件
- newInstance(): 呼叫沒有參數的「public」建構子。
```
Object obj1 = fish_clz.newInstance();
```
- getConstructor(Class... clazz): 參數是Class類別，呼叫類型與數量對映「public」有參數的建構子，參數...是可變參數。
```
Constructor constructor = fish_clz.getConstructor(String.class);
Object obj2 = constructor.newInstance("Bill");
```
- getDecalaredConstructor(Class... clazz): 
參數是Class類別，呼叫類型與數量對映「public 或 private」有參數的建構子，參數...是可變參數。
```
// 使用private私有 有參數 建構子
Constructor constructor1 = fish_clz.getDeclaredConstructor(String.class, int.class);
// 修改存取權限變成public
constructor1.setAccessible(true);
Object obj3 = constructor1.newInstance("Mary", 10);
```

魚的類別
{% highlight java linenos %}
class Fish {
  public String name;
  private int age;

  public Fish() {}

  public Fish(String name) {
    this.name = name;
  }

  // 私有
  private Fish(String name, int age) {
    this.age = age;
    this.name = name;
  }

  @Override
  public String toString() {
    return "Fish{" +
        "name='" + name + '\'' +
        ", age=" + age +
        '}';
  }
}
{% endhighlight %}

完整程式碼
{% highlight java linenos %}
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class ReflectTest4 {
  public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
    // 取得class
    Class fish_clz = Class.forName("reflect.Fish");
    // 使用public 無參數 建構子 建立物件
    Object obj1 = fish_clz.newInstance();
    System.out.println("obj1 = " + obj1);
    // 使用public 有參數 建構子 建立物件
    Constructor constructor = fish_clz.getConstructor(String.class);
    Object obj2 = constructor.newInstance("Bill");
    System.out.println("obj2 = " + obj2);
    // 使用private私有 有參數 建構子
    Constructor constructor1 = fish_clz.getDeclaredConstructor(String.class, int.class);
    // 修改存取權限變成public
    constructor1.setAccessible(true);
    Object obj3 = constructor1.newInstance("Mary", 10);
    System.out.println("obj3 = " + obj3);
  }
}
{% endhighlight %}
```
obj1 = Fish{name='null', age=0}
obj2 = Fish{name='Bill', age=0}
obj3 = Fish{name='Mary', age=10}
```

## 反射取得成員變數
以下有public、private、static成員變數。
{% highlight java linenos %}
class Fish {
  // public
  public String name = "Default";
  // private
  private int age = 0;
  // private + static
  private static int weight = 0;
  public Fish() {
  }

  public Fish(String name) {
    this.name = name;
  }

  private Fish(String name, int age) {
    this.age = age;
    this.name = name;
  }

  @Override
  public String toString() {
    return "Fish{" +
        "name='" + name + '\'' +
        ", age=" + age +
        '}';
  }
}
{% endhighlight %}

取得成員變數的方法:
- getField(): 取得public的成員變數。
- getDeclaredField(): 取得public 與 private成員變數。

設定屬性
```
屬性名.set(物件,值);
age.set(obj1, 30);
```

取得屬性
```
屬性名.get(物件);
age.get(obj1)
```

{% highlight java linenos %}
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;

public class ReflectTest4 {
  public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchFieldException {
    // 取得class
    Class fish_clz = Class.forName("reflect.Fish");
    // 使用public 無參數 建構子 建立物件
    Object obj1 = fish_clz.newInstance();

    // public
    Field name = fish_clz.getField("name");
    System.out.println("name = " + name.get(obj1));
    name.set(obj1, "Alice");
    System.out.println("After changed name = " + name.get(obj1));
    // private
    Field age = fish_clz.getDeclaredField("age");
    age.setAccessible(true);
    System.out.println("age = " + age.get(obj1));
    age.set(obj1, 30);
    System.out.println("After changed age = " + age.get(obj1));
    // private + static
    Field weight = fish_clz.getDeclaredField("weight");
    weight.setAccessible(true);
    System.out.println("weight = " + weight.get(null));
    weight.set(null, 100);
    System.out.println("After changed weight = " + weight.get(null));
  }
}
{% endhighlight %}
```
name = Default
After changed name = Alice
age = 0
After changed age = 30
weight = 0
After changed weight = 100
```

## 反射呼叫成員方法
{% highlight java linenos %}
class Fish {
  public Fish() {
  }
  // public 方法
  public String method1(String name, int age) {
    return "public method = " + name + age;
  }
  // private 方法
  private String method2(String name, int age) {
    return "private method = " + name + age;
  }
  // static 方法
  private static int method3(int age) {
    return age;
  }
}
{% endhighlight %}

### 取得public method 無參數
```
Method 方法變數 = class物件.getMethod("方法名");
Method method1 = fish_clz.getMethod("method1");
```

呼叫方法
```
Object 變數 = 方法變數.invoke(物件);
Object rtn1 = method1.invoke(obj1);
```

{% highlight java linenos %}
class Fish {
  public void method1_public() {
    System.out.println("method1_public");
  }
{% endhighlight %}

{% highlight java linenos %}
// 取得class
Class clazz = Class.forName("reflect.Fish");
Object obj1 = clazz.newInstance();
Method method1 = clazz.getMethod("method1_public");
method1.invoke(obj1);
{% endhighlight %}
```
method1_public
```

### 取得public method
```
Method 方法變數 = class物件.getMethod("方法名", 參數Class物件);
Method method1 = fish_clz.getMethod("method1", String.class, int.class);
```

呼叫方法
```
Object 變數 = 方法變數.invoke(物件, "參數1", 參數2);
Object rtn1 = method1.invoke(obj1, "Mary", 10);
```

Object是return的類型，固定傳回Object父類別。<br>
但執行時期的類型是真正方法定義的類型，以本例來說，對傳回值使用getClass()，會取得傳回值實際執行的類型。<br>
```
System.out.println(rtn1.getClass());
```
執行結果為class java.lang.String

完整程式碼
{% highlight kotlin linenos %}
// 取得class
Class fish_clz = Class.forName("reflect.Fish");
// 使用public 無參數 建構子 建立物件
Object obj1 = fish_clz.newInstance();
// 取得public 方法
Method method1 = fish_clz.getMethod("method1", String.class, int.class);
// 呼叫方法 取得傳回值，傳回值固定Object類型
Object rtn1 = method1.invoke(obj1, "Mary", 10);
// getClass取得傳回值實際類型
System.out.println(rtn1.getClass());
// 輸出結果
System.out.println(rtn1);
{% endhighlight %}
```
class java.lang.String
public method = Mary10
```

### 取得private method
```
Method 方法變數 = class物件.getDeclaredMethod("方法名", 參數Class物件);
Method method2 = fish_clz.getDeclaredMethod("method2", String.class, int.class);
```

使用方法前，要先把private存取權限設為public。
```
method2.setAccessible(true);
```

再如之前一樣，呼叫方法。<br>

完整程式碼
{% highlight java linenos %}
// 取得class
Class fish_clz = Class.forName("reflect.Fish");
// 使用public 無參數 建構子 建立物件
Object obj1 = fish_clz.newInstance();
Method method2 = fish_clz.getDeclaredMethod("method2", String.class, int.class);
method2.setAccessible(true);
Object rtn = method2.invoke(obj1, "Mary", 10);
System.out.println(rtn.getClass());
System.out.println(rtn);
{% endhighlight %}
```
class java.lang.String
private method = Mary10
```

### 取得staic method
語法，呼叫方法的時候，可以把物件變成null，因為static方法是類別方法，不用使用物件也可以呼叫。
```
Object 變數 = 方法變數.invoke(null, "參數1", 參數2);
Object rtn1 = method1.invoke(null, "Mary", 10);
```

{% highlight java linenos %}
public class ReflectTest4 {
  public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchFieldException {
    // 取得class
    Class fish_clz = Class.forName("reflect.Fish");
    // public
    Object obj1 = fish_clz.newInstance();
    Method method3 = fish_clz.getDeclaredMethod("method3", int.class);
    method3.setAccessible(true);
    // 呼叫static方法
    Object rtn = method3.invoke(null,  10);
    System.out.println(rtn.getClass());
    System.out.println(rtn);
  }
}
{% endhighlight %}
```
class java.lang.Integer
10
```

## 透過反射取得類別的結構
使用先前的Fish類別。

### 取得package套件路徑 與 類別名
{% highlight java linenos %}
// 取得class
Class clazz = Class.forName("reflect.Fish");
// 取得package
System.out.println(clazz.getPackage());
// 取得package + 類別名
System.out.println(clazz.getName());
// 取得類別名
System.out.println(clazz.getSimpleName());
{% endhighlight %}
```
reflect
reflect.Fish
Fish
```

### 共用方法
- getName() 可以取得屬性名、方法名、建構子名
- getType() 取得屬性的類型
- getModifiers() 取得存取權限

|存取權限|編號|
|:------|:--:|
|public| 1 |
|private| 2 |
|protected| 4 |
|static| 8 |
|final| 16|

若有二個存取權限，則是相加，以下public是1 \+ staic 8 = 9
```
public static String name;
```

### 取得屬性
父類別是Animal，子類別是Fish。
{% highlight java linenos %}
class Animal {
  public String type;
  private String color;
}
class Fish extends Animal{
  public String name;
  private String age;
  public Fish() {
  }
}
{% endhighlight %}

#### 取得public屬性，包含父類別
除了獲取本身的public成員變數(屬性)，也可以獲取父類別public的成員屬性。
{% highlight java linenos %}
public class ReflectTest4 {
  public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchFieldException {
    // 取得class
    Class clazz = Class.forName("reflect.Fish");
    Field[] fields = clazz.getFields();
    for (Field field : fields) {
      System.out.println("===================");
      System.out.println("屬性名 = " + field.getName());
      System.out.println("屬性類別 = " + field.getType());
      System.out.println("屬性權限 = " + field.getModifiers());
    }
  }
}
{% endhighlight %}
```
===================
屬性名 = name
屬性類別 = class java.lang.String
屬性權限 = 1
===================
屬性名 = type
屬性類別 = class java.lang.String
屬性權限 = 1
```

#### 取得類別public private全部屬性 不包含父類別
{% highlight java linenos %}
// 取得class
Class clazz = Class.forName("reflect.Fish");
Field[] fields = clazz.getDeclaredFields();
for (Field field : fields) {
  System.out.println("===================");
  System.out.println("屬性名 = " + field.getName());
  System.out.println("屬性類別 = " + field.getType());
  System.out.println("屬性權限 = " + field.getModifiers());
}
{% endhighlight %}
```
===================
屬性名 = name
屬性類別 = class java.lang.String
屬性權限 = 1
===================
屬性名 = age
屬性類別 = class java.lang.String
屬性權限 = 2
```

### 取得方法
{% highlight java linenos %}
class Animal {
  public String type;
  private String color;

  public void fmethod1_public() {}

  public void fmethod2_prive() {}
}

class Fish extends Animal {
  public String name;
  private String age;

  public void method1_public() {}

  public void method2_prive() {}
}
{% endhighlight %}

#### 取得public方法(包含父類別)
- getReturnType() 取得方法傳回值類型
- getParameterTypes() 參數類型

{% highlight java linenos %}
public class ReflectTest4 {
  public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchFieldException {
    // 取得class
    Class clazz = Class.forName("reflect.Fish");
    Method[] methods = clazz.getMethods();
    for (Method method : methods) {
      System.out.println("===================");
      System.out.println("方法名 = " + method.getName());
      System.out.println("傳回類別 = " + method.getReturnType());
      System.out.println("存取權限 = " + method.getModifiers());
      Class<?>[] parameterTypes = method.getParameterTypes();
      for (Class<?> parameterType : parameterTypes) {
        System.out.println("參數類型 = " +parameterType);
      }
    }
  }
}
{% endhighlight %}
```
===================
方法名 = method1_public
傳回類別 = void
存取權限 = 1
===================
方法名 = method2_prive
傳回類別 = void
存取權限 = 1
===================
方法名 = fmethod1_public
傳回類別 = void
存取權限 = 1
===================
方法名 = fmethod2_prive
傳回類別 = void
存取權限 = 1
===================
方法名 = equals
傳回類別 = boolean
存取權限 = 1
參數類型 = class java.lang.Object
===================
方法名 = toString
傳回類別 = class java.lang.String
存取權限 = 1
===================
方法名 = hashCode
傳回類別 = int
存取權限 = 257
===================
方法名 = getClass
傳回類別 = class java.lang.Class
存取權限 = 273
===================
方法名 = notify
傳回類別 = void
存取權限 = 273
===================
方法名 = notifyAll
傳回類別 = void
存取權限 = 273
===================
方法名 = wait
傳回類別 = void
存取權限 = 17
參數類型 = long
===================
方法名 = wait
傳回類別 = void
存取權限 = 17
參數類型 = long
參數類型 = int
===================
方法名 = wait
傳回類別 = void
存取權限 = 17
```
#### 取得類別public private方法(不包含父類別)
{% highlight java linenos %}
// 取得class
Class clazz = Class.forName("reflect.Fish");
Method[] methods = clazz.getDeclaredMethods();
for (Method method : methods) {
  System.out.println("===================");
  System.out.println("方法名 = " + method.getName());
  System.out.println("傳回類別 = " + method.getReturnType());
  System.out.println("存取權限 = " + method.getModifiers());
  Class<?>[] parameterTypes = method.getParameterTypes();
  for (Class<?> parameterType : parameterTypes) {
    System.out.println("參數類型 = " +parameterType);
  }
}
{% endhighlight %}
```
===================
方法名 = method1_public
傳回類別 = void
存取權限 = 1
===================
方法名 = method2_prive
傳回類別 = void
存取權限 = 1
```

### 取得建構子(不包含父類別)
{% highlight java linenos %}
class Animal {
  public String type;
  private String color;

  public void fmethod1_public() {}

  public void fmethod2_prive() {}

  public Animal() {}

  public Animal(String type) {}
}

class Fish extends Animal {
  public String name;
  private String age;

  public Fish() {}

  public Fish(String name) {}

  private Fish(String name, int age) {}

  public void method1_public() {}

  public void method2_prive() {}
}
{% endhighlight %}

#### 取得public建構子(不包含父類別)
{% highlight java linenos %}
Class clazz = Class.forName("reflect.Fish");
Constructor[] constructors = clazz.getConstructors();
for (Constructor constructor : constructors) {
  System.out.println("===================");
  System.out.println("建構子名 = " + constructor.getName());
  System.out.println("存取權限 = " + constructor.getModifiers());
  Class<?>[] parameterTypes = constructor.getParameterTypes();
  for (Class<?> parameterType : parameterTypes) {
    System.out.println("參數類型 = " +parameterType);
  }
}
{% endhighlight %}
```
===================
建構子名 = reflect.Fish
存取權限 = 1
參數類型 = class java.lang.String
===================
建構子名 = reflect.Fish
存取權限 = 1
```
#### 取得public private 建構子(不包含父類別)
存取權限，1 是public， 2 是private。
{% highlight java linenos %}
Class clazz = Class.forName("reflect.Fish");
Constructor[] constructors = clazz.getDeclaredConstructors();
for (Constructor constructor : constructors) {
  System.out.println("===================");
  System.out.println("建構子名 = " + constructor.getName());
  System.out.println("存取權限 = " + constructor.getModifiers());
  Class<?>[] parameterTypes = constructor.getParameterTypes();
  for (Class<?> parameterType : parameterTypes) {
    System.out.println("參數類型 = " +parameterType);
  }
}
{% endhighlight %}
```
===================
建構子名 = reflect.Fish
存取權限 = 2
參數類型 = class java.lang.String
參數類型 = int
===================
建構子名 = reflect.Fish
存取權限 = 1
參數類型 = class java.lang.String
===================
建構子名 = reflect.Fish
存取權限 = 1
```

### 取得父類別
{% highlight java linenos %}
class Animal {
  public String type;
  private String color;

  public Animal() {}

  public Animal(String type) {}
}

interface IA {}

interface IB {}

class Fish extends Animal implements IA, IB {
  public String name;
  private String age;

  public Fish() {}

  public Fish(String name) {}

  private Fish(String name, int age) {}

}
{% endhighlight %}

{% highlight java linenos %}
// 取得class
Class clazz = Class.forName("reflect.Fish");
Class super_clazz = clazz.getSuperclass();
System.out.println(super_clazz);
{% endhighlight %}
```
class reflect.Animal
```
### 取得Interface
{% highlight java linenos %}
// 取得class
Class clazz = Class.forName("reflect.Fish");
Class[] interfaces = clazz.getInterfaces();
for (Class anInterface : interfaces) {
  System.out.println(anInterface);
}
{% endhighlight %}
```
interface reflect.IA
interface reflect.IB
```

### 取得Annoations
{% highlight java linenos %}
@Deprecated
class Fish {
  public Fish() {}
}
{% endhighlight %}

{% highlight java linenos %}
Class clazz = Class.forName("reflect.Fish");
Annotation[] annotations = clazz.getAnnotations();
for (Annotation annotation : annotations) {
  System.out.println(annotation);
}
{% endhighlight %}
```
@java.lang.Deprecated(forRemoval=false, since="")
```

## 測試的程式碼
{% highlight java linenos %}
interface Fly {
  void fly();
}

interface Swim {
  void swim();
}

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

由執行結果發現，會執行到 Initialization 初始化
{% highlight java linenos %}
Class class1 = Class.forName("reflect.Duck");
System.out.println(class1);
{% endhighlight %}
```
Duck 被classloader載入
class reflect.Duck
```

## 類別名.class 取得 Class 物件

static 區塊：
```
static {
    System.out.println("Duck 被classloader載入");
}
```
實際上是在 初始化階段 執行的，而不是載入階段。

Duck.class 只做「載入」，不做「初始化」

```
Duck.class
```

類別檔被 class loader 載入

類別資訊被讀取

Class 物件被建立

但 不會執行 static 區塊。

```
因為 JVM 規範明確說：

類別名.class 取得 Class 物件並不會觸發 class initialization。
```
{% highlight java linenos %}
Class class2 = Duck.class;
System.out.println(class2);
{% endhighlight %}
```
class reflect.Duck
```

## getClass()
由執行結果發現，會執行到 Initialization 初始化
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
[4]: {% link _pages/java/class.md %}