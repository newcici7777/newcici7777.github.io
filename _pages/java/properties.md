---
title: Properties
date: 2025-11-21
keywords: java, Properties
---
## Properties 介紹
Properties的父類別是Hashtable。
```
java.lang.Object
  java.util.Dictionary<K,V>
    java.util.Hashtable<Object,Object>
      java.util.Properties
```
Properties是一個類別。

使用`Key=Value`儲存。<br>

注意！Key中間不能為空白，Value不能因為它是String，就使用`"文字"`雙引號。<br>

## 常用方法
- load 載入資源檔
- list 將內容顯示到那裡
- getProperty(key) 取得屬性
- setProperty(key) 設定屬性
- stroe 儲存資源檔

## 建立Properties
在src目錄下按滑鼠右鍵。<br>
![img]({{site.imgurl}}/java/properties1.png)<br>

![img]({{site.imgurl}}/java/properties2.png)<br>

內容輸入
```
user=Bill
password=1234
```

## 建立Properties物件
{% highlight java linenos %}
Properties prop = new Properties();
{% endhighlight %}

windows的檔案路徑
```
src\\Test.properties
```

mac、linux的檔案路徑
```
src/Test.properties
```

讀取檔案
{% highlight java linenos %}
Properties prop = new Properties();
prop.load(new FileReader("src/Test.properties"));
{% endhighlight %}

## 取得屬性
{% highlight java linenos %}
public class Property {
  public static void main(String[] args) throws IOException {
    Properties prop = new Properties();
    prop.load(new FileReader("src/Test.properties"));
    String user = prop.getProperty("user");
    String password = prop.getProperty("password");
    System.out.println("user = " + user);
    System.out.println("password = " + password);
  }
}
{% endhighlight %}
```
user = Bill
password = 1234
```

## 設定屬性
語法
```
prop.setProperty("key","value");
```
{% highlight java linenos %}
Properties prop = new Properties();
prop.setProperty("user","Mary");
prop.setProperty("password","abcd");
{% endhighlight %}

屬性設完後，要儲存檔案。
```
prop.store(new FileOutputStream("檔案位置"), "註解");
prop.store(new FileOutputStream("檔案位置"), null);
```
若沒註解，可設為null。

完整程式碼
{% highlight java linenos %}
import java.io.FileReader;
import java.io.IOException;
import java.util.Properties;

public class Property {
  public static void main(String[] args) throws IOException {
    Properties prop = new Properties();
    prop.load(new FileReader("src/Test.properties"));
    String user = prop.getProperty("user");
    String password = prop.getProperty("password");
    System.out.println("user = " + user);
    System.out.println("password = " + password);
    prop.setProperty("user","Mary");
    prop.setProperty("password","abcd");
    user = prop.getProperty("user");
    password = prop.getProperty("password");
    System.out.println("user = " + user);
    System.out.println("password = " + password);
    prop.store(new FileOutputStream("src/Test.properties"), null);
  }
}
{% endhighlight %}
```
user = Bill
password = 1234
user = Mary
password = abcd
```

修改過後的檔案內容。
```
#Fri Nov 21 11:27:15 CST 2025
password=abcd
user=Mary
```

## 建立Properties檔案
語法
```
prop.store(new FileOutputStream("檔案位置"),"註解")
prop.store(new FileOutputStream("檔案位置"), null)
```
若沒註解，可設為null。

建立Properties檔案
{% highlight java linenos %}
Properties prop = new Properties();
prop.setProperty("password","1234");
String comment = "create by cici";
prop.store(new FileOutputStream("src/Test2.properties"),comment);
{% endhighlight %}

產生的檔案內容<br>
```
#create by cici
#Fri Nov 21 11:23:42 CST 2025
password=1234
```