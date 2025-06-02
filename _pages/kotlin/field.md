---
title: set get
date: 2025-05-21
keywords: kotlin, field, get, set
---
## 屬性位置
屬性是放在類別中。
```
class 類別名 {
  var 屬性名: 屬性類型 = 初始值
  val 屬性名: 屬性類型 = 初始值

  // 屬性是空值類型，可指派空值，空值類型是var，因為要修改null值
  var 屬性名: 屬性類型? = null
}
```

## field get() set(value)
Kotlin會自動為屬性產生field, get(), set(value)方法。

{% highlight kotlin linenos %}
var name = "Alex"
{% endhighlight %}

預設會自動變成下面的程式碼。
{% highlight kotlin linenos %}
var name = "Alex"
    get() = field  
    set(value) {
        field = value    
    }
{% endhighlight %}

### field
field儲存屬性值在記憶體中，如果要使用get()讀取屬性值，或用set()更新屬性值，需要使用field來讀取或更新，field只能在get()方法跟set()方法使用。

為什麼要使用field？因為Java都要自己寫set(), get(), this.屬性 = 參數，this用的太多，簡化成用field省略大量程式碼。

### 覆寫get()方法
get()是讀取屬性，先前的內容說明get()會自動產生，但也可以自己覆寫。

覆寫get()方法，field代表name，把name轉成全大寫。

覆寫語法1
```
var 變數: 類型 = 初始值
    get() {
      // 要記得加上return
      return 回傳值
    }
```

{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() {
            return field?.uppercase()
        }        
}
{% endhighlight %}

覆寫語法2
```
var 變數: 類型 = 初始值
    get() = field
```

{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() = field?.uppercase()
}
{% endhighlight %}

### 覆寫set(value)方法
set(value)，是更新屬性，先前的內容說明set()會自動產生，但也可以自己覆寫。

語法，value為傳進來的參數。
```
var 變數: 類型 = 初始值
    set(value) {
      field = value    
    }
```

trim()去掉前後空白，再把value指派到field，field代表name屬性。
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() = field?.uppercase()
        set(value) {
            field = value?.trim()
        }
}
{% endhighlight %}

### var與val
var才能修改屬性，才會有set()更新方法。

val只能讀取，不能修改，不會有set()更新方法，只有get()讀取。

### set(value)只能用field
set(value)方法中，不能用屬性名稱。

value參數指派給屬性名(例如name)，程式碼就會進入無限迴圈。
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() = field?.uppercase()
        set(value) {
            // 不能這樣寫，不能把value指派給屬性名
            name = value?.trim()
        }
}
{% endhighlight %}

### 覆寫可單獨只寫set()或get()
不用同時覆寫set()與get()

以下只覆寫set()
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        set(value) {
            field = value?.trim()
        }
}
{% endhighlight %}

### 測試
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() = field?.uppercase()
        set(value) {
            field = value?.trim()
        }
}
fun main() {
    var student = Student()
    student.name = "     Bill      "
    println(student.name)
}
{% endhighlight %}
```
BILL
```

### 不使用field覆寫get()
建立一個randomId屬性，每次都取得不同的值，不使用field，也是可以覆寫get()
{% highlight kotlin linenos %}
class Student {
    val randomId
        get() = (1 .. 100).shuffled().first()  // 取出亂數
}
fun main() {
    var student = Student()
    println(student.randomId)
}
{% endhighlight %}
```
22
```

### set()作為檢查的機制
指派給 speakerVolume 屬性的值介於 0 到 100 之間。

可以在 set() 函式中使用 in 關鍵字，並在後面加上值的範圍，以檢查 Int 值是否介於 0 到 100 的範圍內。如果該值在預期範圍內，系統會更新 field 值，否則屬性的值維持不變。
{% highlight kotlin linenos %}
var speakerVolume = 2
    set(value) {
        if (value in 0..100) {
            field = value
        }
    }
{% endhighlight %}

## 存取權限
若set前面加上private，代表不允許其它地方設值，只有本身的類別可以設值。
{% highlight kotlin linenos %}
class Cat4 (_name: String, _age: Int) {
    var name:String = ""
        private set(value) {
            field = value
        }
    var age:Int = 0
    init {
        name = _name
        age = _age
    }
}
fun main() {
    val cat4 = Cat4("喵喵", 2)
    println(cat4.name)
    println(cat4.age)
    // 以下會編譯失敗
    cat4.name = "Momo"
}
{% endhighlight %}

## Java原始碼
### var非空值屬性
Student類別中有一個var屬性。
{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
{% endhighlight %}

#### NotNull與checkNotNullParameter()
看產生的java檔，[Decompile轉成Java檔][1]

可以發現自動產生getName()、setName()，\@NotNull是kotlin的Annotation，目的是檢查屬性、傳回值、參數是否為null，Annotation的作用在於檢查，發現是null，產生編譯例外。

checkNotNullParameter()方法是檢查是否為空值。
{% highlight java linenos %}
public final class Student {
   @NotNull
   private String name = "Alice";

   @NotNull
   public final String getName() {
      return this.name;
   }

   public final void setName(@NotNull String var1) {
      // checkNotNullParameter()
      Intrinsics.checkNotNullParameter(var1, "<set-?>");
      this.name = var1;
   }
}
{% endhighlight %}

#### 修改屬性為Bill
{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
fun main() {
    var student = Student()
    student.name = "Bill"
}
{% endhighlight %}

Java程式檔，發現已經自動產生`setName("Bill")`
{% highlight java linenos %}
public final class ClassTestKt {
   public static final void main() {
      Student student = new Student();
      student.setName("Bill");
   }
{% endhighlight %}

### 屬性為空值類型
#### 把name屬性改成空值類型。
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
}
fun main() {
    var student = Student()
    student.name = "Bill"
}
{% endhighlight %}

#### Nullable 與 checkNotNullParameter()
Java程式碼，\@Nullable代表可為空值。
{% highlight java linenos %}
public final class Student {
   @Nullable
   private String name;

   @Nullable
   public final String getName() {
      return this.name;
   }

   public final void setName(@Nullable String var1) {
      this.name = var1;
   }
}
{% endhighlight %}

可為空值類型，產生出來的Java檔，就沒有checkNotNullParameter()方法。
{% highlight java linenos %}
   public static final void main() {
      Student student = new Student();
      student.setName("Bill");
   }
{% endhighlight %}



[1]: {% link _pages/kotlin/intellij.md %}