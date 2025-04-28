---
title: Java程式碼格式
date: 2025-04-28
keywords: Java, java code format
---
Prerequisites:

- [c++程式碼格式][1]

Java程式碼格式跟C++程式碼格式差距「很大」，特別列出。

[Java風格指南](https://google.github.io/styleguide/javaguide.html)

[C++風格指南](https://www.cclo.idv.tw/google-cpp-styleguide-zhTW/formatting.html#class-format)

## 縮排2個空格
> 4.2 Block indentation: +2 spaces
Java與C++一樣是2個空格，不是4個空格！

## 換行
### C++ 風格（Google C++ Style Guide）
原則：空行越少越好  
目標：讓程式碼緊湊，提高螢幕空間利用率，方便追蹤控制流程。  

範例：
{% highlight c++ linenos %}
class MyClass {
 public:
  MyClass() { Init(); }
  void DoSomething() {
    if (condition) {
      // ...
    } else {
      // ...
    }
  }
 private:
  int value_;
};
{% endhighlight %}

### Java 風格（Google Java Style Guide）
原則：適度空行提升可讀性  
目標：讓程式碼結構更清晰，便於閱讀和理解。  

|情境|	空行數量|
|類別宣告與第一個方法之間|	2 行|
|方法與方法之間|	1 行|
|靜態區塊、成員變數、構造方法之間	|1 行|
|註解與程式碼之間	視情況|（通常 1 行）|

範例：
{% highlight java linenos %}
public class Example {
  private static final int CONSTANT = 100;
  // 成員變數與方法之間空 2 行
  
  public Example() {
    // ...
  }
  // 方法之間空 1 行
  
  public void method1() {
    if (condition) {
      // ...
    }
    // 邏輯轉換處可加空行
    else {
      // ...
    }
  }
  
  public void method2() {
    // ...
  }
}
{% endhighlight %}

## 類別成員順序
Java（靜態→實例→方法） 
1. 靜態成員
2. 實例成員
3. 建構子
4. 方法
5. 內部類別	可讀性優先

C++（方法在前，變數在後） 
1. public: 方法
2. protected: 方法
3. private: 方法
4. 成員變數	封裝性優先

### Java 的類別成員擺放順序（Google Java Style Guide)

#### 靜態成員（static）
static final 常數（public → private)  
static 變數（public → private)  
static 初始化區塊（static { ... }）  

#### 實例成員（instance）
public 常數（final）  
private 常數（final）  
public 變數  
private 變數  
實例初始化區塊（{ ... }）  
建構子（Constructors）  
方法（Methods）  
public 方法 → private 方法  
可依功能分組（如 getter/setter 放一起）  
內部類別（Inner Classes）（如果有）  

{% highlight java linenos %}
public class MyClass {
  // 1. 靜態成員
  public static final int PUBLIC_CONSTANT = 1;
  private static final int PRIVATE_CONSTANT = 2;
  private static int staticVar;
  
  static {
      // 靜態初始化區塊
      staticVar = 10;
  }
  
  // 2. 實例成員
  public final int instanceFinalVar = 5;
  private int instanceVar;
  
  {
      // 實例初始化區塊
      instanceVar = 20;
  }
  
  // 3. 建構子
  public MyClass() {}
  public MyClass(int value) { this.instanceVar = value; }
  
  // 4. 方法
  public void publicMethod() {}
  private void privateMethod() {}
  
  // 5. 內部類別（可選）
  private static class InnerClass {}
}
{% endhighlight %}

### C++ 的類別成員擺放順序（Google C++ Style Guide）
public:  
建構子（Constructors）  
方法（Methods）

protected:  
方法  

private:  
方法  

成員變數（static → 非 static）

原則：空行越少越好

{% highlight c++ linenos %}
class MyClass {
 public:
    // 1. 建構子
    MyClass();
    explicit MyClass(int value);
    // 2. 公開方法
    void PublicMethod();
 protected:
    // 3. 保護方法（可選）
    void ProtectedMethod();
 private:
    // 4. 私有方法
    void PrivateMethod();
    // 5. 成員變數（最後）
    static int static_var_;
    int instance_var_;
};
{% endhighlight %}

## 結論
程式碼風格確實存在許多分歧，甚至可能讓人覺得「過度糾結」。

過度糾結風格確實浪費時間

如果團隊在「空行數量」或「縮排用空格還是 Tab」爭論半天，這些時間本可用來解決實際問題。

使用工具自動化，避免手動檢查，爭吵空行與縮排是沒有意義的。


[1]: {% link _pages/c/basic/format.md %}

