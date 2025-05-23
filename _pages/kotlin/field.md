---
title: 屬性
date: 2025-05-21
keywords: kotlin, field, get, set
---
## 類別body
類別被\{\}花括號包住。
```
class 類別名 {
  // 類別body
}
```
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

## 自動產生field,get(),set()
Kotlin會自動為屬性產生field變數, get(), set()方法。

## field
field用來儲存屬性，field只能在get()方法跟set()方法使用。

為什麼要使用field？因為Java都要自己寫set(), get(), this.屬性 = 參數，this用的太多，簡化成用field省略大量程式碼。

## var
只有var屬性才能修改，才會有set()方法。

## val
val只能讀取，不能修改，不會有set()方法。

## 屬性非空值類型
### 定義類別Student
定義Student類別，可以在.kt文件裡面建立class
{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
{% endhighlight %}

### NotNull與checkNotNullParameter()
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

### 建立物件
建立物件，並把物件指派給變數。
```
var 變數 = 類別名()
val 變數 = 類別名()
```

建立物件，並把物件指派給student變數。
{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
fun main() {
    // 建立物件
    var student = Student()
}
{% endhighlight %}

### val物件變數
此處的val並不是不能修改物件的屬性，而是不能再把val物件變數指向其它物件。

{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
fun main() {
    // val物件變數
    val student2 = Student()
    // 以下會出錯 因為Val cannot be reassigned
    student2 = Student()
}
{% endhighlight %}

val物件變數，可以修改物件的屬性，以下程式碼把name指派新的值。
{% highlight kotlin linenos %}
class Student {
    var name = "Alice"
}
fun main() {
    val student2 = Student()
    student2.name = "Doris"
    println(student2.name)
}
{% endhighlight %}
```
Doris
```
### 修改屬性為Bill
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

## 屬性空值類型
### 把name屬性改成空值類型。
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
}
fun main() {
    var student = Student()
    student.name = "Bill"
}
{% endhighlight %}

### Nullable
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

### 沒有checkNotNullParameter()
可為空值類型，產生出來的Java檔，就沒有checkNotNullParameter()方法。
{% highlight java linenos %}
   public static final void main() {
      Student student = new Student();
      student.setName("Bill");
   }
{% endhighlight %}

## 覆寫get()方法
覆寫語法1
```
var 變數: 類型 = 初始值
    get() {
    	// 要記得加上return
    	return 回傳值
    }
```

覆寫語法2
```
var 變數: 類型 = 初始值
    get() = 值
```

覆寫get()方法，field代表name，把name轉成全大寫。

覆寫語法1
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() {
            return field?.uppercase()
        }        
}
{% endhighlight %}

覆寫語法2
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() = field?.uppercase()
}
{% endhighlight %}

## 覆寫set()方法
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

## 覆寫可單獨只寫set()或get()
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

## set()只能用field
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

## 測試
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

## 不使用field覆寫get()
建立一個randomId屬性，每次都取得不同的值，不使用field，也是可以覆寫get()
{% highlight kotlin linenos %}
class Student {
    var name: String? = null
        get() = field?.uppercase()
        set(value) {
            field = value?.trim()
        }
    val randomId
        get() = (1 .. 100).shuffled().first()  // 取出亂數
}
fun main() {
    var student = Student()
    student.name = "     Bill      "
    println(student.name)
    println(student.randomId)
}
{% endhighlight %}
```
BILL
22
```

[1]: {% link _pages/kotlin/intellij.md %}