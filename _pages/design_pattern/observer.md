---
title: 觀察者模式
date: 2025-04-30
keywords: Java, Design patterns, Observer Pattern
---
## 需求
台灣氣象站提供一個天氣API，Line與Google會訂閱氣象站，一旦氣溫、溼度的資料更新，會通知有訂閱的網站更新氣象資料。

## 類別圖
![img]({{site.imgurl}}/pattern/observer.png)

Subject主題介面，要實作的方法有register()、remove()、notifyObserver()。

WeatherAPI實作Subject，成員屬性有observers，setData()為設定溫度與溼度的方法。

Observer觀察者介面，要實作的方法update()

Line與Google實作Observer。

## Subject主題
{% highlight java linenos %}
public interface Subject {
  void register(Observer o);
  void remove(Observer o);
  void notifyObserver();
}
{% endhighlight %}

## Observer觀察者
{% highlight java linenos %}
public interface Observer {
  void update(float temperature, float humidity);
}
{% endhighlight %}

## WeatherAPI
{% highlight java linenos %}
public class WeatherAPI implements Subject{
  // 溫度
  private float temperature;
  // 溼度
  private float humidity;
  // 觀察者們
  private List<Observer> observers;

  public WeatherAPI() {
    observers = new ArrayList<>();
  }

  //註冊
  @Override
  public void register(Observer o) {
    observers.add(o);
  }

  // 刪除
  @Override
  public void remove(Observer o) {
    observers.remove(o);
  }

  //通知
  @Override
  public void notifyObserver() {
    for (Observer o: observers) {
      // 通知觀察者們更新氣象資訊
      o.update(getTemperature(),getHumidity());
    }
  }

  public float getTemperature() {
    return temperature;
  }

  public float getHumidity() {
    return humidity;
  }

  public void setData(float temperature, float humidity) {
    this.temperature = temperature;
    this.humidity = humidity;
    // 通知觀察者們
    notifyObserver();
  }
}
{% endhighlight %}

## Google,Line
Google
{% highlight java linenos %}
public class Google implements Observer{
  @Override
  public void update(float temperature, float humidity) {
    System.out.println("=====Google=======");
    System.out.println("temperature: " + temperature);
    System.out.println("humidity: " + humidity);
  }
}
{% endhighlight %}

Line
{% highlight java linenos %}
public class Line implements Observer{
  @Override
  public void update(float temperature, float humidity) {
    System.out.println("=====Line=======");
    System.out.println("temperature: " + temperature);
    System.out.println("humidity: " + humidity);
  }
}
{% endhighlight %}

## Main主程式
{% highlight java linenos %}
public class Test {
  public static void main(String[] args) {
    WeatherAPI weatherAPI = new WeatherAPI();
    // 註冊觀察者們
    weatherAPI.register(new Google());
    weatherAPI.register(new Line());
    // 更新天氣
    weatherAPI.setData(27f, 68f);
  }
}
{% endhighlight %}
```
=====Google=======
temperature: 27.0
humidity: 68.0
=====Line=======
temperature: 27.0
humidity: 68.0
```