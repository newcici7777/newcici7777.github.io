---
title: Java Memory Model
date: 2025-05-14
keywords: Java, Java Memory Model
---
Java Memory Model，中文是記憶體模型，簡體中文是內存模型。

![img]({{site.imgurl}}/java/memory_model.png)

## 基本資料型別
基本資料型別有int, char, float, double, boolean，字母以小寫開頭。

基本資料型別的值是放在stack中，以下是放入stack中的過程。

![img]({{site.imgurl}}/java/assign_int1.png)

![img]({{site.imgurl}}/java/assign_int2.png)

![img]({{site.imgurl}}/java/assign_int3.png)

## 陣列記憶體位址複製

陣列是物件，「物件」都是放在「Heap」空間，「Stack」放的是「變數」，「變數」的值是存放「Heap」空間的「記憶體位址」。

![img]({{site.imgurl}}/java/assign_arr1.png)

![img]({{site.imgurl}}/java/assign_arr2.png)

![img]({{site.imgurl}}/java/assign_arr3.png)

## 陣列複製
遇到new關鍵字，就是在Heap中建立一個記憶體空間，空間大小根據物件類型所定義，此處定義大小為3。

Stack中的變數，存放「Heap」空間的「記憶體位址」。
![img]({{site.imgurl}}/java/assign_arrcopy1.png)

箭頭指向for{}最後，代表已經執行完for迴圈，Heap中的記憶體位址中的值也被設成跟arr1的值是一樣的。
![img]({{site.imgurl}}/java/assign_arrcopy2.png)

## 2維陣列
陣列存的是1維陣列的記憶體位址。
![img]({{site.imgurl}}/java/assign_arr2d1.png)

![img]({{site.imgurl}}/java/assign_arr2d2.png)

## 物件建立過程
Dog類別
{% highlight java linenos %}
class Dog {
  public String name;
  public int age;
  public String color;

  public Dog (String name, int age, String color) {
    this.name = name;
    this.age = age;
    this.color = color;
  }
}
{% endhighlight %}

建立Dog
{% highlight java linenos %}
  Dog white_dog = new Dog("小白",5,"白色");
  Dog yellow_dog = new Dog("小黃",6,"黃色");
  Dog black_dog = new Dog("小黑",7,"黑色");
{% endhighlight %}

Class Loader載入類別資訊

![img]({{site.imgurl}}/java/obj_model1.png)

new關鍵字，在Heap建立記憶體空間0x0070，Stack的white_dog變數存的是0x0070，Heap中0x0070位址中的name與age與color，全用預設值，int基本型別用0，name與color是String物件用null。
![img]({{site.imgurl}}/java/obj_model2.png)

進入建構子，在String pool字串常數池，建立"小白"字串常數記憶體空間，Heap空間的物件name的值設成"小白"的記憶體位址。
![img]({{site.imgurl}}/java/obj_model3.png)

age是int基本型別，直接設數字5。
![img]({{site.imgurl}}/java/obj_model4.png)

color是String類別，在String pool字串常數池，建立"白色"字串常數記憶體空間，物件的color設成"白色"的記憶體位址。
![img]({{site.imgurl}}/java/obj_model5.png)

三個物件建立完畢的記憶體模型
![img]({{site.imgurl}}/java/assign_obj1.png)

164
165
176
194
199
203
466