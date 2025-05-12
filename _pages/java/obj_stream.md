---
title: 序列化與反序列化
date: 2025-05-12
keywords: Java, ObjectInputStream, ObjectOutputStream, Serializable
---
Prerequisites:
- [ByteArray串流][1]
- [裝飾串流][2]

## 裝飾串流ObjectInputStream與ObjectOutputStream
ObjectOutputStream 把物件寫入。

ObjectInputStream 讀取物件。

## 寫入基本類型
{% highlight java linenos %}
  public static void main(String[] args) {
    ByteArrayOutputStream bos = null;
    ObjectOutputStream oos = null;
    ObjectInputStream ois = null;
    try {
      // 讀取與寫入的位置是在記憶體緩衝區
      bos = new ByteArrayOutputStream();
      oos = new ObjectOutputStream(bos);
      // 寫入基本資料型態
      // 寫入整數
      oos.writeInt(100);
      // 寫入布林
      oos.writeBoolean(true);
      // 寫入字元
      oos.writeChar('A');
      // 寫入浮點數
      oos.writeDouble(5.99);
      // 寫入字串，是使用writeUTF()
      oos.writeUTF("測試");

      // 讀取物件
      ois = new ObjectInputStream(new ByteArrayInputStream(bos.toByteArray()));
      // 要按照寫入順序讀取
      System.out.println(ois.readInt());
      System.out.println(ois.readBoolean());
      System.out.println(ois.readChar());
      System.out.println(ois.readDouble());
      System.out.println(ois.readUTF());

    } catch (IOException e) {
      e.printStackTrace();
    } catch (ClassNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
      	// 只需要關閉裝飾串流，詳見裝飾串流文章
        if (oos != null)
          oos.close();
        if (ois != null)
          ois.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
{% endhighlight %}
```
100
true
A
5.99
測試
```

## 將物件寫到串流
### 物件都要implements Serializable
Student
{% highlight java linenos %}
public class Student implements Serializable {
  private String name;
  private int age;
  // 成員變數family是類別
  private Family family;

  public Student() {
    family = new Family();
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }
  public String getMother() {
    return family.getMother();
  }

  public void setMother(String mother) {
    family.setMother(mother);
  }

  public String getFather() {
    return family.getFather();
  }

  public void setFather(String father) {
    family.setFather(father);
  }
}
{% endhighlight %}

### 成員變數類別也要implements Serializable
Family是Student的成員變數類別，也要implements Serializable
{% highlight java linenos %}
public class Family implements Serializable {
  private String mother;
  private String father;

  public String getMother() {
    return mother;
  }

  public void setMother(String mother) {
    this.mother = mother;
  }

  public String getFather() {
    return father;
  }

  public void setFather(String father) {
    this.father = father;
  }
}
{% endhighlight %}

### 程式碼
{% highlight java linenos %}
  public static void main(String[] args) {
    ByteArrayOutputStream bos = null;
    ObjectOutputStream oos = null;
    ObjectInputStream ois = null;
    try {
      bos = new ByteArrayOutputStream();
      oos = new ObjectOutputStream(bos);
      // 寫入基本資料型態
      oos.writeInt(100);
      oos.writeBoolean(true);
      oos.writeChar('A');
      oos.writeDouble(5.99);
      // 寫入字串
      oos.writeUTF("測試");

      // 寫入物件
      Student student = new Student();
      student.setName("小美");
      student.setAge(10);
      student.setMother("小美媽");
      student.setFather("小美爸");
      oos.writeObject(student);

      // 讀取物件
      ois = new ObjectInputStream(new ByteArrayInputStream(bos.toByteArray()));
      // 要按照寫入順序讀取
      System.out.println(ois.readInt());
      System.out.println(ois.readBoolean());
      System.out.println(ois.readChar());
      System.out.println(ois.readDouble());
      System.out.println(ois.readUTF());
      // readObject()是Object物件
      // 向下轉型成Student才能使用Student的方法
      Student copy = (Student) ois.readObject();
      System.out.println(copy.getName());
      System.out.println(copy.getAge());
      System.out.println(copy.getFather());
      System.out.println(copy.getMother());
    } catch (IOException e) {
      e.printStackTrace();
    } catch (ClassNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
      	// 只需要關閉裝飾串流，詳見裝飾串流文章
        if (oos != null)
          oos.close();
        if (ois != null)
          ois.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
{% endhighlight %}
```
100
true
A
5.99
測試
小美
10
小美爸
小美媽
```

## Deep Clone程式碼
- [Deep Clone][3]

{% highlight java linenos %}
  // 建立byte陣列寫出串流
  ByteArrayOutputStream baos = new ByteArrayOutputStream();
  // 為byte陣列輸出串流 加上物件的功能
  // 原文: adds functionality to output stream
  ObjectOutputStream oos = new ObjectOutputStream(baos);
  // 把物件寫入byte陣列
  // 使用ObjectOutputStream.writeObject()
  oos.writeObject(object);
  
  // 建立byte陣列讀取串流， 資料的來源是byte陣列
  ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());
  // 為byte陣列輸入串流 加上物件的功能
  // 原文: adds functionality to input stream
  ObjectInputStream ois = new ObjectInputStream(bais);
  // 把物件讀取出來
  // 使用ObjectInputStream.readObject()
  return (T) ois.readObject();
{% endhighlight %} 

[1]: {% link _pages/java/bytearr_io.md %}
[2]: {% link _pages/java/decorator_io.md %}
[3]: {% link _pages/design_pattern/deepclone.md %}