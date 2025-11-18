---
title: 委托 by Delegation
date: 2025-11-11
keywords: kotlin, by, Delegation
---
## 類別委托
Kotlin的委托，我覺得十分像設計模式中的[代理人模式][1]。

就是旅客跟旅行社買機票，但實際付錢是旅客付。

{% highlight kotlin linenos %}
// BuyTicket介面 要做的事
interface BuyTicket {
    fun pay()
}

// 旅客實作BuyTicket介面的pay方法。
// 真正要做事的人
class Tourist : BuyTicket {
    override fun pay() = println("旅客提供信用卡 卡號付錢")
}

// 旅行社實作BuyTicket介面的pay方法，實際上是旅客付錢。
// 代理人
class Agency(tourist: BuyTicket) : BuyTicket by tourist
{% endhighlight %}

{% highlight kotlin linenos %}
fun main() {
    // 真正要做事的人
    val tourist = Tourist()
    // 代理人，把 真正要做事的人 的傳入
    val agency = Agency(tourist)
    // 實際上是旅客付錢，不是代理人付錢
    agency.pay()
}
{% endhighlight %}
```
旅客提供信用卡 卡號付錢
```

以上的內容要有Java程式碼對映才比較明白。<br>

BuyTicket介面
{% highlight java linenos %}
public interface BuyTicket {
  void pay();
}
{% endhighlight %}

旅客實作BuyTicket介面的pay方法。
{% highlight java linenos %}
public class Tourist implements BuyTicket{
  @Override
  public void pay() {
    System.out.println("旅客提供信用卡 卡號付錢");
  }
}
{% endhighlight %}

旅行社實作BuyTicket介面的pay方法，實際上是旅客付錢。
{% highlight java linenos %}
public class Agency implements BuyTicket{
  // 成員屬性要有旅客Tourist
  private Tourist tourist;

  // 使用建構子參數，把旅客傳入旅行社
  public Agency(Tourist tourist) {
    this.tourist = tourist;
  }

  @Override
  public void pay() {
    // 實際上是呼叫旅客的pay()方法
    tourist.pay();
  }
}
{% endhighlight %}

Client測試
{% highlight java linenos %}
public class Client {
  public static void main(String[] args) {
    // 建立旅客
    Tourist tourist = new Tourist();
    // 把旅客傳入旅行社的建構子
    Agency agency = new Agency(tourist);
    // 旅行社付錢，實際上旅客付的。
    agency.pay();
  }
}
{% endhighlight %}
```
旅客提供信用卡 卡號付錢
```

語法
```
class 代理人(真正要做事的人: 要做的事) : 要做的事 by 真正要做事的人
class Agency(tourist: BuyTicket) : BuyTicket by tourist
```

## 屬性委托
要先了解operator是什麼才能繼續往下。<br>

- [operator plus][4]
- [operator by][2]

Kotlin 的 by 委託，是用來把「屬性的 getter / setter 行為」交給另一個物件處理。

語法
```
var 屬性名   by 委托類別
var address by someDelegate
```

使用 `by` 委托關鍵字，委托的類別要實作setValue()與getValue()方法。<br>

- getValue：當你讀取屬性時會被呼叫
- setValue：當你寫入屬性時會被呼叫（只針對 var，val 沒有 setValue）

getValue
```
operator fun getValue(thisRef: Any?, property: KProperty<*>): T
```
- thisRef	擁有這個屬性的物件實例（如果是 companion object 就是類別）
- property	屬性本身的描述資訊，例如 name 或 age
- 回傳值	屬性的值

setValue
```
operator fun setValue(thisRef: Any?, property: KProperty<*>, value: T)
```
- thisRef	擁有這個屬性的物件實例
- property	屬性描述資訊
- value	新的值

{% highlight kotlin linenos %}
import kotlin.reflect.KProperty

class Student {
    var address: String by AddressDelegate()
}

class AddressDelegate {
    // 需要有一個暫存變數temp，儲存變數的值
    // 變數預設值為no data
    private var temp = "no data"
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        println("$thisRef ,讀取屬性 ${property.name}")
        // get()的時候，傳回暫存變數
        return temp
    }
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String){
        // 設定暫存變數temp
        temp = value
        println("$thisRef , 設定屬性 ${property.name} change to $value")
    }
}

fun main() {
    val student = Student()
    // call AddressDelegate getValue()
    println(student.address)
    // call AddressDelegate setValue()
    student.address = "Taiwan"
    // call AddressDelegate getValue()
    println(student.address)
}
{% endhighlight %}
```
no data
learn2.Student@4783da3f , 設定屬性 address change to Taiwan
learn2.Student@4783da3f , 讀取屬性 address
Taiwan
```
### thisRef 
thisRef 與 property 讓 delegate 可以知道「屬性屬於哪個物件」以及「屬性名稱」

### getValue setValue
- [operator by][2]
語法
```
by 委托物件
```
委托的物件，必須實作以下二個屬性。
```
operator fun getValue → 當你讀取屬性時呼叫

operator fun setValue → 當你寫入屬性時呼叫
```
by 就是把屬性的「存取行為」委託給這兩個函式

## KProperty

- [kotlin反射][6]

`KProperty<*>` 告訴你的 delegate：「你正在操作的屬性叫什麼名字、型別是什麼」

property.name 屬性的名稱

property.returnType 屬性的類型

## Kotlin 標準庫內建的委託
### by Delegates.observable() 屬性變化監聽
可以監聽屬性值改變時的事件。<br>
語法<br>
```
var name:String by Delegates.observable("初始值") {
    property, oldValue, newValue ->
    println("${property.name} from $oldValue change to $newValue")
}
```

{% highlight kotlin linenos %}
import kotlin.properties.Delegates

fun main() {
    var name:String by Delegates.observable("empty") {
        property, oldValue, newValue ->
        println("${property.name} from $oldValue change to $newValue")
    }
    name = "Hello"
    name = "World"
{% endhighlight %}
```
name from empty change to Hello
name from Hello change to World
```

常用於 UI binding 或 資料同步 場景，比如 ViewModel 中監控狀態變化。

### by Delegates.vetoable() 可拒絕的屬性變更
比 observable 多一步「審核機制」，可以決定是否接受新值。<br>
以下newValue必須大於0，小於150，才可以設定新的值。<br>
語法<br>
```
var age:Int by Delegates.vetoable(初始值) {
    property, oldValue, newValue ->
    newValue > 0 && newValue < 150
}
```
{% highlight kotlin linenos %}
fun main() {
    var age:Int by Delegates.vetoable(0) {
        property, oldValue, newValue ->
        newValue > 0 && newValue < 150
    }
    age = 10
    println("age = $age")
    age = -1
    println("age = $age")
    age = 200
    println("age = $age")
}
{% endhighlight %}
```
age = 10
age = 10
age = 10
```
適合用在「狀態限制」或「驗證條件」場景，例如不能設定負值、非法輸入等。

## by 與 二個冒號::的引用

- [二個冒號::引用][5]

### 全域變數
{% highlight kotlin linenos %}
var topLevelValue: String = "初始值"

class Person {
    var name: String by ::topLevelValue
}
{% endhighlight %}
::topLevelValue → 取得「全域變數 topLevelValue 的屬性引用（KProperty）」

by ::topLevelValue → 將 name 的 getter/setter 委託給這個屬性

### 成員變數
{% highlight kotlin linenos %}
class User {
    private var realValue: String = "default"
    var name: String by this::realValue
}
{% endhighlight %}
this::realValue → 取得「本 class 的成員 realValue 的屬性引用」

用在 by 上，讓 name 的讀寫委託給 realValue

### 其它類別成員
{% highlight kotlin linenos %}
class Storage {
    var data: String = "init"
}

class Article(val storage: Storage) {
    var title: String by storage::data
}
{% endhighlight %}
storage::data → 取得 storage 物件的 data 成員的引用

Article.title 的 getter/setter 會交給 storage.data

### by與 兩個冒號`::`使用時機
#### 做「資料別名」（alias）或「映射」

有時你想讓 class 的屬性只是「另一個地方的值的別名」。

例如：
{% highlight kotlin linenos %}
var coreName: String = "SystemCore"

class AppInfo {
    var name: String by ::coreName
}
{% endhighlight %}

等於 AppInfo.name 指向 coreName。
像「參考 / alias」，但用 Kotlin 語法實現。

#### 想把資料集中管理，而不是分散在每個 class

有時候一筆資料應該放在「全域」，但希望 class 裡面用起來像自己的屬性。

例：
{% highlight kotlin linenos %}
var globalSetting: String = "default"

class Settings {
    var theme: String by ::globalSetting
}

{% endhighlight %}


使用端：
{% highlight kotlin linenos %}
val s = Settings()
println(s.theme)
{% endhighlight %}

看起來像：

s.theme


但本質是：

globalSetting

#### 多個類別想共用同一筆資料

如果多個 class 都要共享一個值，例如：

全域狀態

設定值（App config）

使用者登入資訊

全域計數器

你可以把該數據放在全域變數，再讓多個物件用委託方式共用它。

範例
{% highlight kotlin linenos %}
var globalToken: String = "init"

class A {
    var token: String by ::globalToken
}
class B {
    var token: String by ::globalToken
{% endhighlight %}

A().token = "123"
會直接改到 B 的 token。

這用法最常見於：
- 全域設定
- 多類別共享資料
- 全域狀態管理


[1]: {% link _pages/design_pattern/proxy.md %}
[2]: {% link _pages/kotlin/operator_by.md %}
[3]: {% link _pages/java/reflect.md %}
[4]: {% link _pages/kotlin/operator_plus.md %}
[5]: {% link _pages/kotlin/refer_operator.md %}
[6]: {% link _pages/kotlin/kotlin_reflect.md %}
