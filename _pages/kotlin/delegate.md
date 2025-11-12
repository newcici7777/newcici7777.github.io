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
- [set get][2]
語法
```
var 屬性名   by 委托類別
var address by someDelegate
```
預設 var 屬性會有set() get()，透過`by`關鍵字，把set()與get()讓其它類別去實作。<br>

其它類別要實作setValue()與getValue()，並不是實作set()與get()方法。<br>

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
operator fun getValue → 當你讀取屬性時呼叫

operator fun setValue → 當你寫入屬性時呼叫

by 就是把屬性的「存取行為」委託給這兩個函式

### KProperty 
- [反射][3]

Heap 儲存Class類別的屬性(但實際上是讀取Meta Space的FieldInfo)

Meta Space儲存類別的屬性FieldInfo。<br>

KProperty 為 屬性（Property）的完整描述
```
java.lang.reflect.Field + java.lang.reflect.Method
```
由反射的屬性與方法組成。<br>

KProperty 是 Kotlin 反射 (kotlin.reflect) 套件的物件，
裡面記錄了這個屬性對應到：

- 哪個類別 (declaringClass)
- 名稱 (name)
- getter/setter 方法
- 是否是 var 或 val
- 註解（annotations）


在 Kotlin 裡，`KProperty<*>` 是 反射屬性類型（Kotlin Property Reflection），
它代表你正在讀取或寫入的那個 屬性本身。

`KProperty<*>` 告訴你的 delegate：「你正在操作的屬性叫什麼名字、型別是什麼」

property.name 屬性的名稱，本例是`address`

property.returnType 屬性的類型，本例是 String

類型符號 `KProperty<*>` 的意義

KProperty<T> → 泛型 T 表示屬性的型別

例如 KProperty<String> → 代表 String 屬性

`KProperty<*>` → 泛型未知，用在 delegate 時最方便

因為 delegate 可以作用在任意型別的屬性上

所以 * 就像 Java 的 ?（通配符），表示「任意型別的屬性」

## Kotlin 標準庫內建的委託
### by Delegates.observable() —— 屬性變化監聽
可以監聽屬性值改變時的事件。
{% highlight kotlin linenos %}
import kotlin.properties.Delegates

var name: String by Delegates.observable("未設定") { prop, old, new ->
    println("${prop.name} 從 $old 改為 $new")
}

fun main() {
    name = "Alice"
    name = "Bob"
}
{% endhighlight %}
```
name 從 未設定 改為 Alice
name 從 Alice 改為 Bob
```

常用於 UI binding 或 資料同步 場景，比如 ViewModel 中監控狀態變化。

### by Delegates.vetoable() —— 可拒絕的屬性變更
比 observable 多一步「審核機制」，可以決定是否接受新值。
{% highlight kotlin linenos %}
var score: Int by Delegates.vetoable(0) { _, old, new ->
    new >= 0 // 負數就拒絕變更
}
{% endhighlight %}

適合用在「狀態限制」或「驗證條件」場景，例如不能設定負值、非法輸入等。

[1]: {% link _pages/design_pattern/proxy.md %}
[2]: {% link _pages/kotlin/field.md %}
[3]: {% link _pages/java/reflect.md %}