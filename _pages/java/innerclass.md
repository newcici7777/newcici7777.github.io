---
title: 內部類別
date: 2025-04-18
keywords: Java, Inner class
---
## 內部類別Inner class
在一個類別中，宣告一個類別，這個類別就是內部類。

## 外部類別Outer Class
包含內部類別的類別稱為外部類別。
{% highlight java linenos %}
public class Outter { //外部類別
  public class inner{ // 內部類別
    
  }
}
{% endhighlight %}

## 外部類別的成員變數與成員方法
外部類別的成員變數與成員方法，在內部類別是可以存取。
{% highlight java linenos %}
public class Outter {
  private String name = "Outter";
  public void doSomething() {}
  public class inner{
    void print() {
      doSomething(); // 呼叫外部類別方法
      System.out.println("outter name = " + name); // 使用外部類別屬性
    }
  }
}
{% endhighlight %}

## 內部類別是外部類別的成員
外部類別可以宣告一個內部類別的成員變數
{% highlight java linenos %}
public class Outter {
  private Inner inner; // 成員屬性是內部類別
  public class Inner{
    void print() {}
  }
}
{% endhighlight %}

## 外部類別可以用new建立內部類別
將成員變數指向內部類別「物件」。
{% highlight java linenos %}
public class Outter {
  private Inner inner; // 成員屬性是內部類別
  public class Inner{
    void print() {}
  }
  public Outter() {
    // 建立Outter物件的時候，建立內部類別
    inner = new Inner();
  }
}
{% endhighlight %}

## 外部類別可以使用內部類的方法
{% highlight java linenos %}
public class Outter {
  private Inner inner; // 成員屬性是內部類別
  public class Inner{
    void print() {}
  }
  public Outter() {
    // 建立Outter物件的時候，建立內部類別
    inner = new Inner();
    inner.print(); // 外部類別使用內部類別的方法
  }
}
{% endhighlight %}

## 內部類別可以有public、private、protected、預設(只能在同一個package下才能用)
內部類別權限設為private，代表只有Outter這個外部類別可以存取，其它類別不能讀取到PrivateInner這個類別。
{% highlight java linenos %}
public class Outter {
  private class PrivateInner {
    void display() {
      System.out.println("Private inner class");
    }
  }

  public class PublicInner {
    void display() {
      System.out.println("Public inner class");
    }
  }
}
{% endhighlight %}

## 在類別之外建立內部類別
{% highlight java linenos %}
class Test {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.PublicInner pub = outer.new PublicInner(); // 可以存取
        pub.display();
        
        outer.createInner(); // 間接存取 private 內部類別
    }
}
{% endhighlight %}

## 在類別之外可以間接存取private內部類別

{% highlight java linenos %}
public class Outer {
    private class PrivateInner {
        void display() {
            System.out.println("Private inner class");
        }
    }
    public PrivateInner createInner() {
        PrivateInner pri = new PrivateInner();  // 可以存取
        return new PrivateInner();
    }
}
{% endhighlight %}

{% highlight java linenos %}
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.PrivateInner inner1 = outer.createInner();
        inner1.display();  // 可以呼叫
        
        // 以下程式碼會編譯錯誤，因為 PrivateInner 是 private 的
        // Outer.PrivateInner inner2 = outer.new PrivateInner();
    }
}
{% endhighlight %}

## 區域內部類別
- 定義在方法或代碼塊中的類別
- 不能使用 public、private 或 protected 修飾
- 只能被該方法或代碼塊內的代碼存取

{% highlight java linenos %}
public class Outer {
    public void someMethod() {
        class LocalInner {  // 不能加 public/private/protected
            // 類別內容
        }
    }
}
{% endhighlight %}

### 內部類別不能有Static變數與Static方法

## 其它內部類別文章:

- [靜態內部類別][1]


[1]: {% link _pages/java/static.md %}










