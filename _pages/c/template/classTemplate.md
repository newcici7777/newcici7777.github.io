---
title: 模板類
date: 2024-04-22
keywords: c++, template
---
## 語法  
```
模板定義    模板形參定義 形參通用名字 逗號 
template < class         T1       ,   class T2>
```
模板定義以關鍵字template開始，後接模板形參表（template parameter list），模板形參是以class開頭，後接形參的通用名字(T1)。模板形參表是用尖括號括住的一個或者多個模板形參的列表，形參之間以逗號分隔。class也可以用typename互相替換。

## 定義模板類
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
第1行，template為模板定義，<class K, class V>模板形參定義(type parameter)。  
第2行，模板函式的名稱為MyMap。  
第5行，建構子，初始私有成員key與value。  
第6行，取得私有成員key。  
第9行，取得私有成員value。  
第13行，私有成員key。  
第14行，私有成員value。  
## 使用模板類
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
第2行，建立模板類物件，用尖括號<K,V>  指定模板形參類型，K的類型為int，V的類型為string，呼叫傳進二個參數的建構子。  
第3行，印出值。  
第4行，印出值。  
第6行，使用指針建立模板類物件，用尖括號<K,V>  指定模板形參類型，K的類型為string，V的類型為string，使用new的方式建立指針，(
"abc","def")呼叫二個參數的建構子。  
第7行，使用->印出值。  
第8行，使用->印印出值。  