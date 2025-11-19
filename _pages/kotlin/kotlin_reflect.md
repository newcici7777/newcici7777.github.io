---
title: kotlin 反射
date: 2025-11-18
keywords: kotlin, reflect
---
Prerequisites:

- [Java 反射][1]
- [兩個冒號::引用][2]

類別是 KClass

方法是 KFunction

欄位是 KProperty

## KClass

getter / setter 是自動產生

主建構子 / 次建構子概念

data class: componentN、自動 equals、hashCode

sealed class

suspend function（會被編譯成不同樣子）

inline class

typealias

companion object（Java 不存在這東西）

Java 的 Class 無法描述這些行為，所以 Kotlin 需要 KClass。

## KProperty 
KProperty 為 屬性（Property）的完整描述

KProperty 是 Kotlin 反射 (kotlin.reflect) 套件的物件，
裡面記錄了這個屬性對應到：

- 哪個類別 (declaringClass)
- 名稱 (name)
- getter/setter 方法
- 是否是 var 或 val
- 註解（annotations）

在 Kotlin 裡，`KProperty<*>` 是 反射屬性類型（Kotlin Property Reflection），
它代表你正在讀取或寫入的那個 屬性本身。

類型符號 `KProperty<*>` 的意義

KProperty<T> → 泛型 T 表示屬性的型別

例如 KProperty<String> → 代表 String 屬性

`KProperty<*>` → 泛型未知，用在 delegate 時最方便

因為 delegate 可以作用在任意型別的屬性上

所以 * 就像 Java 的 ?（通配符），表示「任意型別的屬性」

## Java Reflection Kotlin KClass 對照表

簡單對映表

|Java Reflection	|Kotlin KClass 對應 API|
|getSuperclass()	|supertypes|
|getInterfaces()	|java.interfaces 或 supertypes 過濾|
|getFields()	|memberProperties|
|getDeclaredFields()	|declaredMemberProperties|
|getMethods()	|memberFunctions|
|getDeclaredMethods()	|declaredMemberFunctions|

1.取得 Class / KClass

|功能|	Java|	Kotlin|
|取得 class 物件|	Duck.class|	Duck::class（KClass）|
|動態載入類別|	Class.forName("Duck")|	Class.forName("Duck").kotlin（KClass)|

KClass轉成Java Class
```
Duck::class.java（Class）
```

java寫法，取得Class
```
類別名.class
```

kotlin 取得KClass
```
類別名::class
```

kotlin 取得Java Class
```
類別名::class.java
```

2.父類別與介面

|功能|	Java|	Kotlin|
|父類別|	clazz.getSuperclass()|	kClass.supertypes|
|介面|	clazz.getInterfaces()|	kClass.java.interfaces 或 supertypes 過濾 interface|

3.取得欄位（Fields）

|功能| Java| Kotlin |
|public 欄位| clazz.getFields()| kClass.memberProperties|
|所有欄位（含 private)| clazz.getDeclaredFields() | kClass.declaredMemberProperties |
|欄位名稱| field.getName()|property.name|

4.取得方法（Methods）

|功能|	Java|	Kotlin|
|public 方法|clazz.getMethods()|	kClass.memberFunctions|
|所有方法（含 private）|clazz.getDeclaredMethods()|kClass.declaredMemberFunctions|
|取得特定方法|clazz.getDeclaredMethod("x")|`declaredMemberFunctions.first { it.name == "x" }`|

5.建構子（Constructors）

|功能|Java|Kotlin|
|public 建構子|	clazz.getConstructors()	|`kClass.constructors.filter { it.visibility == PUBLIC }`|
|所有建構子（含 private）|	clazz.getDeclaredConstructors()	|kClass.constructors（KClass 一律包含全部）|
|取出無參數建構子|clazz.getDeclaredConstructor()|`kClass.constructors.first { it.parameters.isEmpty() }`|
|呼叫建構子|constructor.newInstance()|constructor.call()|

6.修改存取權限（private → public）

|功能|	Java|	Kotlin|
|設置 private| 可呼叫	method.setAccessible(true)|method.isAccessible = true|
|建構子private|constructor.setAccessible(true)|	constructor.isAccessible = true |

7.呼叫方法

|功能|	Java|	Kotlin|
|呼叫 public 方法|	method.invoke(instance)|	method.call(instance)|
|呼叫 private 方法|	method.setAccessible(true) |method.isAccessible = true|
|呼叫|method.invoke(instance)|method.call(instance)|
|呼叫帶參數方法|method.invoke(obj, a, b)|method.call(obj, a, b)|

8.建立物件

|功能|	Java|	Kotlin|
|以預設建構子建立|	clazz.newInstance()|	kClass.createInstance()|
|以特定建構子建立|	constructor.newInstance(args...)|	constructor.call(args...)|

9.取得屬性與方法的詳細資訊

|功能|	Java|	Kotlin|
|取得參數型別|	method.getParameterTypes()|	function.parameters|
|取得回傳型別|	method.getReturnType()|	function.returnType|
|欄位型別	|field.getType()|	property.returnType|
|檢查是否是 private|	Modifier.isPrivate(method.getModifiers())|	function.visibility|

10.父類別方法 or 屬性

java
```
clazz.getSuperclass().getDeclaredMethods();
```

kotlin
```
val superClass = kClass.supertypes.first().classifier as KClass<*>
superClass.declaredMemberFunctions
```

## 反射範例
{% highlight kotlin linenos %}
open class Animal {
    val species: String = "unknown"
}

interface Flyable
interface Swimable

class Duck : Animal(), Flyable, Swimable {
    val name: String = "Donald"

    fun quack() {}
    private fun secret() {}
}
{% endhighlight %}

### 取得父類別
{% highlight kotlin linenos %}
fun main() {
    val kClass = Duck::class
    val superClass = kClass.supertypes

    println("Supertypes:")
    superClass.forEach { println(it) }
}
{% endhighlight %}
```
Kotlin 的 supertypes 會列出：
父類別 Animal
介面 Flyable
介面 Swimable
```

### 取得介面
{% highlight kotlin linenos %}
fun main() {
    val kClass = Duck::class
    println("Interfaces:")
    kClass.java.interfaces.forEach { println(it) }
}
{% endhighlight %}

### 取得public屬性
{% highlight kotlin linenos %}
import kotlin.reflect.full.memberProperties

fun main() {
    val kClass = Duck::class

    println("Public properties:")
    kClass.memberProperties.forEach { println(it) }
}
{% endhighlight %}


### 取得所有屬性
{% highlight kotlin linenos %}
import kotlin.reflect.full.declaredMemberProperties

fun main() {
    val kClass = Duck::class

    println("Declared properties (all from this class):")
    kClass.declaredMemberProperties.forEach { println(it) }
}
{% endhighlight %}

### 取得public方法
{% highlight kotlin linenos %}
import kotlin.reflect.full.memberFunctions

fun main() {
    val kClass = Duck::class

    println("Public methods:")
    kClass.memberFunctions.forEach { println(it) }
}
{% endhighlight %}

### 取得所有方法
{% highlight kotlin linenos %}
import kotlin.reflect.full.declaredMemberFunctions

fun main() {
    val kClass = Duck::class

    println("Declared methods (all):")
    kClass.declaredMemberFunctions.forEach { println(it) }
}
{% endhighlight %}

### 取得父類別所有方法
{% highlight kotlin linenos %}
import kotlin.reflect.full.declaredMemberFunctions

open class Animal {
    fun speak() {}
    private fun secret() {}
}

class Dog : Animal() {
    fun bark() {}
}

fun main() {
    val kClass = Dog::class

    // 子類別
    println("Declared methods in Dog:")
    kClass.declaredMemberFunctions.forEach { println(it) }

    // 父類別
    val superKClass = kClass.supertypes.first().classifier as? KClass<*>
    println("Declared methods in Animal:")
    superKClass?.declaredMemberFunctions?.forEach { println(it) }
}

{% endhighlight %}

[1]: {% link _pages/java/reflect.md %}
[2]: {% link _pages/kotlin/refer_operator.md %}
