---
title: 列舉
date: 2025-06-09
keywords: Java, enum
---
Prerequisites:

- [Static][1]
- [靜態屬性是建構子][2]
- [final][3]
- [建構子][4]

## 類別作為列舉
以下程式碼是用類別做的enum，需要有以下條件。
1. 建構子全為私有，不可讓其它類別使用new建立。
2. 屬性使用public \+ final \+ static \+ 本身類別 屬性大寫 \= new 本身類別建構子\(\);
3. 建構子的參數都為private，可以提供get，不能提供set。
4. 屬性跟著類別的生命周期，類別被[載入][5]時就建立。
{% highlight java linenos %}
public class Gender {
  public final static Gender BOY = new Gender("男");
  public final static Gender GIRL = new Gender("女");
  private String name;

  private Gender() {
  }

  private Gender(String name) {
    this.name = name;
  }

  @Override
  public String toString() {
    return name;
  }
}
{% endhighlight %}

使用方法
```
類別.靜態屬性
```
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(Gender.BOY);
    System.out.println(Gender.GIRL);
  }
}
{% endhighlight %}
```
男
女
```

## Enum
改為enum，把class改為enum。
{% highlight java linenos %}
public enum Gender1 {}
{% endhighlight %}

把以下這段程式碼。
{% highlight java linenos %}
public final static Gender BOY = new Gender("男");
{% endhighlight %}

改為以下程式碼，都是要放在程式碼最前面，二者都是呼叫一個參數的建構子，只是隱藏public \+ final \+ static 。
{% highlight java linenos %}
public enum Gender1 {
  BOY("男"); // 放在最前面
}
{% endhighlight %}

若有多個屬性，以逗號`,`分隔，以分號`;`結尾。
{% highlight java linenos %}
public enum Gender1 {
  BOY("男"), GIRL("女");
}
{% endhighlight %}

{% highlight java linenos %}
public enum Gender1 {
  BOY("男"), GIRL("女");
  private String name;

  private Gender1() {
  }

  private Gender1(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }

  @Override
  public String toString() {
    return name;
  }
}
{% endhighlight %}

使用方法
```
類別.靜態屬性
```
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(Gender1.BOY);
    System.out.println(Gender1.GIRL);
  }
}
{% endhighlight %}
```
男
女
```
## 無建構子參數
1. 沒有參數就省略圓括號`()`。
2. 沒有參數就省略私有建構子，會自動產生。
3. BOY與GIRL都是final靜態屬性，屬性類型就是Gender2。
{% highlight java linenos %}
public enum Gender2 {
  BOY,GIRL;
}
{% endhighlight %}

使用時，沒有參數的建構子，會印出靜態屬性名。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    System.out.println(Gender2.BOY);
    System.out.println(Gender2.GIRL);
  }
}
{% endhighlight %}
```
BOY
GIRL
```

## Enum的toString()與name()
在[Object][7]文章有提過印出物件，也是呼叫toString()的方法，但Enum有對toString()改寫。

在程式碼輸入`Enum`，win使用ctrl \+ b，mac使用cmd \+ b，進入原始碼。

預設使用name來toString()，若建構子沒參數，name就是屬性名，如上個程式碼`Gender2.BOY`，屬性名是BOY。
{% highlight java linenos %}
public abstract class Enum<E extends Enum<E>>
        implements Constable, Comparable<E>, Serializable {
    private final String name;

	public final String name() {
        return name;
    }

    public String toString() {
        return name;
    }
}
{% endhighlight %}

## static屬性
使用上一個程式碼，請問建立二個物件，都是使用Gender2.BOY建立的，這二個物件是相等嗎？
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Gender2 boy1 = Gender2.BOY;
    Gender2 boy2 = Gender2.BOY;
    System.out.println(boy1 == boy2);
  }
}
{% endhighlight %}
```
true
```
由執行結果可以發現，屬性是static，都是指向相同[靜態記憶體空間][6]。

## ordinal()
印出屬性的順序，預設是從0開始。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Gender2 boy = Gender2.BOY;
    Gender2 girl = Gender2.GIRL;
    System.out.println(boy.ordinal());
    System.out.println(girl.ordinal());
  }
}
{% endhighlight %}
```
0
1
```
## values()
傳回所有屬性。
{% highlight java linenos %}
public enum Gender2 {
  BOY,GIRL;
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Gender2[] arr = Gender2.values();
    for (Gender2 val: arr) {
      System.out.println(val);
    }
  }
}
{% endhighlight %}
```
BOY
GIRL
```

## valueOf()
使用字串屬性名，取出靜態屬性物件。
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Gender2 boy = Gender2.valueOf("BOY");
    System.out.println(boy);
  }
}
{% endhighlight %}
```
BOY
```
若屬性名亂寫，會拋出Error。
{% highlight java linenos %}
Gender2 boy = Gender2.valueOf("ABC");
{% endhighlight %}

java.lang.IllegalArgumentException:No enum constant enum1.Gender2.ABC

## compareTo()
Enum原始檔中的compareTo，比較的是ordinal。
{% highlight java linenos %}
public final int compareTo(E o) {
    Enum<?> other = o;
    Enum<E> self = this;
    if (self.getClass() != other.getClass() && // optimization
        self.getDeclaringClass() != other.getDeclaringClass())
        throw new ClassCastException();
    return self.ordinal - other.ordinal;  // 這行才是重點
}
{% endhighlight %}

{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Gender2 boy = Gender2.BOY;    // ordinal 0
    Gender2 girl = Gender2.GIRL;  // ordinal 1
    System.out.println(boy.compareTo(girl));
  }
}
{% endhighlight %}
```
-1
```

## Enum與javap反編譯
在Intellij用終端機
![img]({{site.imgurl}}/java/enum_decompile.png)

輸入`javap 檔名.class`，以下是反編譯的結果，可以看到`Gender2 extends java.lang.Enum`
```
% ls  
Gender.class    Gender1.class   Gender2.class   Hobby.class     Test.class
% javap Gender2.class
Compiled from "Gender2.java"
public final class enum1.Gender2 extends java.lang.Enum<enum1.Gender2> implements enum1.Hobby {
  public static final enum1.Gender2 BOY;
  public static final enum1.Gender2 GIRL;
  public static enum1.Gender2[] values();
  public static enum1.Gender2 valueOf(java.lang.String);
  public void hobby();
  static {};
}
```

## Enum與介面
因為Enum類別已經繼承Enum，由上一個javap已經證實。

Enum類別不能再繼承其它類別，只能實作介面。

以下是興趣的介面。
{% highlight java linenos %}
interface Hobby {
  void hobby();
}
{% endhighlight %}

實作Hobby介面。
{% highlight java linenos %}
public enum Gender2 implements Hobby {
  BOY,GIRL;

  @Override
  public void hobby() {
    if (this.name() == BOY.name()) {
      System.out.println("大部分喜歡玩電動、棒球、運動。");
    } else {
      System.out.println("大部分喜歡購物、追劇。");
    }
  }
}
{% endhighlight %}

測試
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    Gender2 boy = Gender2.BOY;
    Gender2 girl = Gender2.GIRL;
    boy.hobby();
    girl.hobby();
  }
}
{% endhighlight %}
```
大部分喜歡玩電動、棒球、運動。
大部分喜歡購物、追劇。
```


[1]: {% link _pages/java/static.md %}
[2]: {% link _pages/java/codeblock.md %}#靜態屬性是建構子
[3]: {% link _pages/java/final.md %}
[4]: {% link _pages/java/constructor.md %}
[5]: {% link _pages/java/classloader.md %}
[6]: {% link _pages/java/metadata.md %}#存放靜態值
[7]: {% link _pages/java/object.md %}