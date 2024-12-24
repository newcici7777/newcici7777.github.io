---
title: 類別模板
date: 2024-04-22
keywords: c++, template
---
## 語法  
```
宣告模板  宣告資料型態參數  資料型態參數名  逗號 
template < class     T1     ,   class T2>
```
宣告模板從關鍵字template開始，後接資料型態參數列表（template parameter list），資料型態參數是以class開頭，後接資料型態參數名(T1)。資料型態參數列表是用尖括號括住的一個資料型態參數或者多個資料型態參數，資料型態參數之間以逗號分隔。class也可以用typename互相替換。

## 宣告類別模板
{% highlight c++ linenos %}
template <class K, class V>
class MyMap{
public:
  MyMap(){}
  MyMap(K key1, V value1):key(key1),value(value1){}
  K getKey(){
    return key;
  }
  V getValue(){
    return value;
  }
private:
  K key;
  V value;
};
{% endhighlight %}
第1行，template為類別模板宣告，<class K, class V>資料型態參數宣告(type parameter)。  

第2行，類別模板的名稱為MyMap。  

第5行，建構式，初始私有成員key與value。  

第6行，取得私有成員key。  

第9行，取得私有成員value。  

第13行，私有成員key。  

第14行，私有成員value。  

## 使用類別模板
{% highlight c++ linenos %}
int main() {
  MyMap<int,string> myMap(3, "test");
  cout << "myMap key=" << myMap.getKey() << endl;
  cout << "myMap value=" << myMap.getValue() << endl;
  
  MyMap<string, string> *myMap_P1 = new MyMap<string,string>("abc","def");
  cout << "myMap_P1 key=" << myMap_P1->getKey() << endl;
  cout << "myMap_P1 value=" << myMap_P1->getValue() << endl;
    return 0;
}
{% endhighlight %}
第2行，建立物件，用尖括號<K,V>  指定參數資料型態，K的資料型態為int，V的資料型態為string，呼叫二個參數的建構式。  

第3行，印出值。  

第4行，印出值。  

第6行，動態配置建立物件，用尖括號<K,V>  指定參數資料型態，K的資料型態為string，V的資料型態為string，使用new動態配置建立物件，(
"abc","def")呼叫二個參數的建構式。  

第7行，使用->印出值。  

第8行，使用->印出值。  
