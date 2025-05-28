---
title: 多型
date: 2025-04-17
keywords: Java, polymorphism, vtable
---
## 編譯類型 與 執行類型
編譯類型，英文是Compile-time type。

執行類型，英文是Runtime type。

什麼是編譯類型？什麼是執行類型？

下方的程式碼，等號左邊是編譯類型，等號右邊是執行類型。

{% highlight java linenos %}
編譯類型     =  執行類型
Parent parent = new Child();
{% endhighlight %}

說白話一點就是，雖然類型是父類別，但實際上指向的是子類別物件。

多型就是，物件的「類型是父類別」，但實際上是用「子類別」的「建構子建立」物件。

## Memory Layout
Prerequisites:

- [Object Layout core][1]

- [基本型態與Wrapper Classes][2]

下面的程式碼有Animal、Dog，Dog繼承Animal。
{% highlight java linenos %}
class Animal {
  int i = 10;  // 父類欄位
  void speak() { 
    System.out.println(i); 
  }
}

class Dog extends Animal {
  int i = 5;  // 子類同名欄位
}
{% endhighlight %}

Dog Memory Layout如下:
```
記憶體開始位置/ 佔記憶體大小/ 型別/ 變數/ 存放的值
OFF  SZ   TYPE DESCRIPTION               VALUE
  0   8        (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4        (object header: class)    0x010033f8 Dog方法記憶體位址
 12   4    int Animal.i                  10
 16   4    int Dog.i                     5
 20   4        (object alignment gap)    
物件大小Instance size: 24 bytes

```

發現Dog物件中有二個變數，一個變數值為10，來自父類別Animal.i，另一個變數值為5是自己的Dog.i。
```
記憶體開始位置/ 佔記憶體大小/ 型別/ 變數/ 存放的值
OFF  SZ   TYPE DESCRIPTION               VALUE
 12   4    int Animal.i                  10
 16   4    int Dog.i                     5
```

## 反射

- [取得父類別所有方法][3]

使用反射知道子類繼承那些方法。

## javap
使用javap知道執行時物件是誰？執行的方法是來自那個物件。

Dog繼承Animal，執行時使用多型，變數類型是父類Animal，指向Dog物件。
{% highlight java linenos %}
class Animal {
    public void speak() { System.out.println("Animal sound"); }
    public void eat() { System.out.println("Animal eating"); }
}

class Dog extends Animal {
    @Override 
    public void speak() { System.out.println("Bark!"); }
    public void fetch() { System.out.println("Fetching..."); }
}

public class Test1 {
    public static void main(String[] args) throws Exception {
        Animal a = new Dog();  // 多型
        a.speak();             // 輸出: Bark!
        Thread.sleep(5 * 60 * 1000); // 讓程序一直執行
    }
}
{% endhighlight %}

編譯以上程式碼
```
javac Test1.java
```

執行javap
```
javap -c -v Test1 
```

實際呼叫的是Dog()建構子，但使用父類別Animal.speak()。
```
   #3 = Methodref          #2.#20      //  Dog.<init>:()V
   #4 = Methodref          #22.#23     // Animal.speak:()V

  #20 = NameAndType        #10:#11     //  "<init>":()V
  #21 = Utf8               Dog
  #22 = Class              #29         //  Animal
  #23 = NameAndType        #30:#11     //  speak:()V

  0: new           #2                  // class Dog
  4: invokespecial #3                  // Method Dog."<init>":()V
  9: invokevirtual #4                  // Method Animal.speak:()V
```

## vtable
在Memory Layout中，從8byte開始到11byte結束，佔了4個byte記憶體空間，存放的是vtable的記憶體位址，什麼是vtable？

儲存Dog物件的所有方法。

以下表格第4行為vtable的記憶體位置。

Dog Memory Layout如下:
```
記憶體開始位置/ 佔記憶體大小/ 型別/ 變數/ 存放的值
OFF  SZ   TYPE DESCRIPTION               VALUE
  0   8        (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4        (object header: class)    0x010033f8  <- Dog物件的所有方法 記憶體位址
 12   4    int Animal.i                  10
 16   4    int Dog.i                     5
 20   4        (object alignment gap)    
物件大小Instance size: 24 bytes

```

由於呼叫vtable語法太困難了，以下的vtable是AI模擬出來。

從下面可以發現，Dog有三個方法，分別是equals(), hashCode(), speak()。
```
vtable for Dog:
[0] Object.equals()@addr1    # 繼承自 Object
[1] Object.hashCode()@addr2  # 繼承自 Object
[2] Animal.speak()@addr3     # 繼承自 Animal
```

由於Dog沒有覆寫父類別的speak()方法，因為在vtable中，speak()方法是來自父類別Animal。

因此以下程式碼執行時，會去看Dog的vtable中的speak()方法，然後才知道要去呼叫Animal.speak()，因為是呼叫Animal類別，所以使用的是Animal類別中的i屬性，印出的結果是10。
{% highlight java linenos %}
class Animal {
  int i = 10;  // 父類欄位
  void speak() { 
    System.out.println(i); 
  }
}

class Dog extends Animal {
  int i = 5;  // 子類同名欄位
}
{% endhighlight %}

{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    Animal myPet = new Dog();  // 多型
    myPet.speak();  // 輸出：10（非 5！）
  }
}
{% endhighlight %}

## 覆寫之後的vtable
若是Dog類別有覆寫speak()方法，vtable裝的是什麼？

{% highlight java linenos %}
class Dog extends Animal {
  int i = 5;  // 子類同名欄位
  void speak() { System.out.println(i); }
}
{% endhighlight %}

```
vtable for Dog:
[0] Animal.equals()@123456    # 繼承自 Object
[1] Animal.hashCode()@234567  # 繼承自 Object
[2] Dog.speak()@345678        
```
由上面可以發現，vtable\[2\]已經變成Dog.speak()。

執行以下程式碼，結果為5，因為是呼叫Dog類別，所以使用的是Dog類別中的i屬性。
{% highlight java linenos %}
public class Test2 {
  public static void main(String[] args) {
    Animal myPet = new Dog();  // 多型
    myPet.speak();
  }
}
{% endhighlight %}

## 父類別轉型子類別
### 自動轉型
子類別轉父類別只會有一個父類別，因此使用自動轉型。

{% highlight java linenos %}
// 以下是自動轉型
Animal myPet = new Dog();
{% endhighlight %}

### 強制轉型
父類別下面會有多個子類別，不確定要轉成那個子類別，因此採用強制轉型，也就是使用括號，括號中是要轉型的(子類別)。
{% highlight java linenos %}
Animal myPet = new Dog();
// 強制轉型成子類別
Dog dog = (Dog) myPet;
{% endhighlight %}

強轉回子類別就可以用子類別的屬性與方法。


[1]: {% link _pages/java/obj_layout_core.md %}
[2]: {% link _pages/java/wrap.md %}
[3]: {% link _pages/java/reflect.md %}#取得父類別所有方法