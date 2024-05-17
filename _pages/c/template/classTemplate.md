---
title: 類別模板
date: 2024-04-22
keywords: c++, template
---
## 語法  
```
宣告模板    宣告型別參數  型別參數名    逗號 
template < class         T1       ,   class T2>
```
宣告模板從關鍵字template開始，後接型別參數列表（template parameter list），型別參數是以class開頭，後接型別參數名(T1)。型別參數列表是用尖括號括住的一個型別參數或者多個型別參數，型別參數之間以逗號分隔。class也可以用typename互相替換。

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
第1行，template為類別模板宣告，<class K, class V>型別參數宣告(type parameter)。  

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
第2行，建立物件，用尖括號<K,V>  指定參數型別，K的型別為int，V的型別為string，呼叫二個參數的建構式。  

第3行，印出值。  

第4行，印出值。  

第6行，動態配置建立物件，用尖括號<K,V>  指定參數型別，K的型別為string，V的型別為string，使用new動態配置建立物件，(
"abc","def")呼叫二個參數的建構式。  

第7行，使用->印出值。  

第8行，使用->印印出值。  
