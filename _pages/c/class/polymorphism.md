---
title: 多型
date: 2025-03-14
keywords: c++, polymorphism
---

## 多型概念
所謂的多型就是`父類別的指標指向子類別，並且可以使用子類別覆寫父類別的函式。`

父類別的指標指向子類別時，子類別不用強制轉型成父類別。

以下程式碼，父類別Parent指標指向子類別Child，子類別也同時有func()函式，但使用父類別指標呼叫func()函式則會呼叫父類別的func()。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  void func() {
    cout << "Parent func()" << endl;
  }
};
class Child:public Parent {
 public:
  // 子類別覆寫func()函式
  void func() {
    cout << "Child func()" << endl;
  }
};
int main() {
  // 父類別Parent指標指向子類別Child
  // 子類別不用強制轉型成父類別
  Parent* ptr = new Child;
  // 呼叫父類別的func()
  ptr->func();
  return 0;
}
{% endhighlight %}
```
Parent func()
```

## virtual
為了解決這個問題，C++發明了virtual關鍵字。  
在`宣告`函式的時候，在函式前面加上virtual關鍵字，告訴編譯器，若父類別指標指向子類別時，子類別有覆寫父類別virtual函式，則使用子類別的函式，若子類別沒覆寫則使用父類別的函式。  
以下程式碼父類別有二個virtual函式func1與func2，子類別只覆寫func1。  
當父類別指標指向子類別，父類別指標呼叫func1()，實際上是會去看子類別有沒有覆寫func1()，若有覆寫則使用子類別func1()。  
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  virtual void func1() {
    cout << "Parent func1()" << endl;
  }
  virtual void func2() {
    cout << "Parent func2()" << endl;
  }
};
class Child:public Parent {
 public:
  void func1() {
    cout << "Child func1()" << endl;
  }
};
int main() {
  Parent* ptr = new Child;
  ptr->func1();
  ptr->func2();
  return 0;
}
{% endhighlight %}
```
Child func1()
Parent func2()
```

## 使用父類別的函式
父類別指標指向子類別，子類別有覆寫父類別virtual函式，但仍要使用父類別的函式，則使用父類別::函式()。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  virtual void func1() {
    cout << "Parent func1()" << endl;
  }
  virtual void func2() {
    cout << "Parent func2()" << endl;
  }
};
class Child:public Parent {
 public:
  void func1() {
    cout << "Child func1()" << endl;
  }
};
int main() {
  Parent* ptr = new Child;
  // 父類別::函式()
  ptr->Parent::func1();
  return 0;
}
{% endhighlight %}
```
Parent func1()
```

## 應用
想像一下，我們要做出一個功能，印出狼、魚、貓、狗、人如何吃東西。  
"狼、魚、貓、狗、人"，他們都是生物，他們都會吃，只是每個生物吃的方法不一樣。
{% highlight c++ linenos %}  
int main() {
  // wolf
  Wolf* wolf = new Wolf();
  wolf->eat(); 
  // fish
  Fish* fish = new Fish();
  fish->eat(); 
  // bird
  Bird* bird = new Bird();
  bird->eat();
  // human
  Human* human = new Human();
  human->eat(); 
  // cat
  Cat* cat = new Cat();
  cat->eat(); 
  return 0;
}
{% endhighlight %}

### 多型步驟:  
1. 先抓出他們的共同動作，eat()，建立父類別Animal中有一個eat()函式。 
2. 父類別eat()函式前面加上virtual。  
3. 動物們繼承父類別Animal，然後子類別覆寫eat()函式。  
4. 父類別的指標指向子類別，父類別指標呼叫子類別eat()函式。  

於是寫下以下的程式碼，有父類別(Animal)，子類別(Wolf、Fish、Cat...)，所有子類別覆寫eat()的函式。  
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Animal{
 public:
  virtual void eat(){}
};
class Wolf: public Animal{
 public:
  void eat(){
    cout << "Wolf eat!" << endl;
  }
};
class Fish: public Animal{
 public:
  void eat(){
    cout << "Fish eat!" << endl;
  }
};
class Bird: public Animal{
 public:
  void eat(){
    cout << "Bird eat!" << endl;
  }
};
class Human: public Animal{
 public:
  void eat(){
    cout << "Human eat!" << endl;
  }
};
class Cat: public Animal{
 public:
  void eat(){
    cout << "Cat eat!" << endl;
  }
};
int main() {
  // wolf
  Animal* wolf = new Wolf();
  wolf->eat();
  // fish
  Animal* fish = new Fish();
  fish->eat();
  // bird
  Animal* bird = new Bird();
  bird->eat();
  // human
  Animal* human = new Human();
  human->eat();
  // cat
  Animal* cat = new Cat();
  cat->eat();
  return 0;
}
{% endhighlight %}

### 相同父類別的子類別全放進vector

- [iterator][1]

我們把這些動物丟進一個籃子中，然後再一個個抓出來，印出"吃"這個動作。  
代表Animal可以有多種類型，可以是Wolf、Fish、Cat...  
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
class Animal{
 public:
  virtual void eat(){}
};
class Wolf: public Animal{
 public:
  void eat(){
    cout << "Wolf eat!" << endl;
  }
};
class Fish: public Animal{
 public:
  void eat(){
    cout << "Fish eat!" << endl;
  }
};
class Bird: public Animal{
 public:
  void eat(){
    cout << "Bird eat!" << endl;
  }
};
class Human: public Animal{
 public:
  void eat(){
    cout << "Human eat!" << endl;
  }
};
class Cat: public Animal{
 public:
  void eat(){
    cout << "Cat eat!" << endl;
  }
};
int main() {
  vector<Animal*> animals;
  animals.push_back(new Wolf());
  animals.push_back(new Fish());
  animals.push_back(new Bird());
  animals.push_back(new Human());
  animals.push_back(new Cat());
  //使用iterator
  vector<Animal*>::iterator it = animals.begin();
  //animals.end()指向最後一個元素的下一個元素
  for(;it != animals.end(); it++) {
    (*it)->eat() ;
  }
  return 0;
}
{% endhighlight %}
```
Wolf eat!
Fish eat!
Bird eat!
Human eat!
Cat eat!
```

## 多型解構子
所謂的多型就是父類別的指標指向子類別，那如果使用父類別的指標進行解構子，會呼叫子類別的解構子嗎？  
以下的執行結果可以發現，不會執行子類別的解構子  
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  ~Parent() {
    cout << "Parent 解構子" << endl;
  }
};
class Child:public Parent {
 public:
  ~Child() {
    cout << "Child 解構子" << endl;
  }
};
int main() {
  Parent* ptr = new Child;
  delete ptr;
  return 0;
}
{% endhighlight %}
```
Parent 解構子
```

為了處理上述問題，若子類別的解構子中有處理記憶體釋放的程式碼，父類別的解構子前面就一定要加上virtual，這樣使用父類別的指標，delete時就會呼叫子類別的解構子然後再呼叫父類別的解構子。

- 銷毀子類別物件，呼叫子類別解構子->呼叫父類別解構子

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  virtual ~Parent() {
    cout << "Parent 解構子" << endl;
  }
};
class Child:public Parent {
 public:
  ~Child() {
    cout << "Child 解構子" << endl;
  }
};
int main() {
  Parent* ptr = new Child;
  delete ptr;
  return 0;
}
{% endhighlight %}
```
Child 解構子
Parent 解構子
```

## 純虛擬函式pure virtual function
在Java中pure virtual function就是抽象方法，c++中文稱為純虛擬函式。  
意思是父類別不實作這個函式，但繼承這個父類別的子類別一定要實作(implementation)此函式。  

- virtual function虛擬函式，通常是撰寫基本的功能，而繼承的子類別則實作(implementation)虛擬函式成為更適用於子類別的。
- pure virtual function純虛擬函式，完全沒有任何基本程式碼的函式，但繼承的子類別一定要實作(implementation)純虛擬函式。
- 父類別中有pure virtual function純虛擬函式，代表這個類別為抽象類別，不能用new建立物件。  
- 父類別中有pure virtual function純虛擬函式，為抽象類別，但可作為父類別指標，實際上指向子類別。(多型)

純虛擬函式pure virtual function語法
```
virtual 傳回值類型 函式(參數) = 0;
virtual void func() = 0;
```

父類別中有pure virtual function純虛擬函式，代表這個類別為抽象類別，不能用new建立物件。  
以下程式碼會編譯錯誤。
{% highlight c++ linenos %}
class Parent {
 public:
  virtual void func() = 0;
};
int main() {
  Parent* ptr = new Parent;
  delete ptr;
  return 0;
}
{% endhighlight %}

如果子類別沒有實作(implementation)父類別的純虛擬函式，也不能用new建立物件。  
以下程式碼會編譯錯誤。
{% highlight c++ linenos %}
class Parent {
 public:
  virtual void func() = 0;
};
class Child:public Parent {
};
int main() {
  Child* ptr = new Child;
  delete ptr;
  return 0;
}
{% endhighlight %}

子類別實作(implementation)父類別的純虛擬函式，會編譯成功。  
父類別參考與父類別指標都是相同的意思，參考的本質是指標。
{% highlight c++ linenos %}
class Parent {
 public:
  virtual void func() = 0;
};
class Child:public Parent {
 public:
  void func() {
    cout << "實作父類別func()" << endl;
  }
};
int main() {
  Child child;
  // 父類別指標指向子類別
  Parent* ptr = &child;
  ptr->func();
  // 父類別參考指向子類別
  Parent &ref1 = child;
  ref1.func();
  return 0;
}
{% endhighlight %}
```
實作父類別func()
```

### 純虛擬函式解構子

原理跟多型解構子一樣。  
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Parent {
 public:
  virtual void func() = 0;
  virtual ~Parent() {
    cout << "父類別解構子" << endl;
  }
};
class Child:public Parent {
 public:
  void func() {
    cout << "實作父類別func()" << endl;
  }
  ~Child() {
    cout << "子類別解構子" << endl;
  }
};
int main() {
  Parent* ptr = new Child;
  ptr->func();
  delete ptr;
  return 0;
}
{% endhighlight %}
```
實作父類別func()
子類別解構子
父類別解構子
```

## typeid判斷類型

使用typeid的==與!=，可以判斷類型。

語法
```
typeid(類型)
typeid(指標)
typeid(參考)
typeid(變數)
```

### 普通類別判斷類型
以下範例建立Student物件s1。  
建立Student指標，建立Student參考。  
註解中有解釋各種比較。  
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
 public:
  string name;
};
int main() {
  Student s1;
  Student* ptr = &s1;
  Student& ref = s1;
  // 判斷s1變數是不是Student類別
  if (typeid(s1) == typeid(Student)) cout << "s1 == Student" << endl;
  // ptr指標，使用*取值運算子，取出指標存放的物件，判斷這個物件是不是Student
  if (typeid(*ptr) == typeid(Student)) cout << "*ptr == Student" << endl;
  // 指標與Student類別，就不是相等，一個是指標，一個是類別，怎麼會是相同東西。
  if (typeid(ptr) == typeid(Student)) cout << "ptr指標 == Student" << endl;
  // 指標與Student指標相比，二者都是指標
  if (typeid(ptr) == typeid(Student*)) cout << "ptr指標 == Student指標" << endl;
  // ref參考是不是Student類別
  if (typeid(ref) == typeid(Student)) cout << "ref == Student" << endl;
  return 0;
}
{% endhighlight %}
```
s1 == Student
*ptr == Student
ptr指標 == Student指標
ref == Student
```

### 多型判斷類型
由於多型的過程，父類別指標指向各種不同的子類別，如先前的例子，Animal可以有多種類型，可以是Wolf、Fish、Cat...  
那麼要如何知道父類別指標是那個子類型呢？透過typeid可以知道。

- 注意！不能把父類別指標與子類別指標進行比較，因為二者是不同的。
- 使用\*取值運算子，將父類別指標指向的記憶體位址的值(物件)取出來，比較類別。

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
class Animal{
 public:
  virtual void eat(){}
};
class Fish: public Animal{
 public:
  void eat(){
    cout << "Fish eat!" << endl;
  }
};
class Cat: public Animal{
public:
  void eat(){
    cout << "Cat eat!" << endl;
  }
};
int main() {
  Animal* animal = new Fish;
  // animal指標是Animal，Fish指標是Fish，二者不是相同的指標
  // 不能用父類別指標與子類別指標相互比較，即便父類別指標指向的記憶體位址的值是子類別
  if (typeid(animal) == typeid(Fish*)) {
    cout << "1.這是魚" << endl;
  } else {
    cout << "1.這不是魚" << endl;
  }
  // 在多型中，不能用指標來判斷類型
  // 要使用指標指向的物件，物件與物件進行比較，才能判斷出類型
  // 使用*取值運算子，父類別指標指向的記憶體位址的值取出來，也就是把子類別物件取出來
  // 判斷物件是不是相同類別
  if (typeid(*animal) == typeid(Fish)) {
    cout << "2.這是魚" << endl;
  } else {
    cout << "2.這不是魚" << endl;
  }
  return 0;
}
{% endhighlight %}
```
1.這不是魚
2.這是魚
```

## dynamic_cast
dynamic_cast主要用於多型，所謂的多型，也就是父類別指標指向子類別記憶體位址。  
當要把父類別指標(實際上指向子類別)轉型成子類別指標，除了原本c語言的強制轉型的方法之外，C++提供了dynamic_cast，把父類別指標轉回原本的子類別指標。  

dynamic_cast轉型失敗會傳回nullptr。

c++轉型語法
```
子類別指標* 指標變數名 = dynamic_cast<子類別指標*>(父類別指標);
Fish* fish_ptr = dynamic_cast<Fish*>(parent_ptr);
```

c語言強制轉型語法  
功能與dynamic_cast相同，但轉型失敗不會傳回nullptr。
```
子類別指標* 指標變數名 = (子類別指標*)父類別指標;
Fish* fish_ptr = (Fish*)parent_ptr;
```

以下的範例是判斷是Fish類別，就呼叫游泳的功能swim()，父類別Animal是沒有swim()函式，必須將父類別指標轉成子類別指標，才能呼叫swim()函式。  
步驟如下:  
1. 使用\*取值運算子取出父類別指標指向的記憶體位址存放的物件。
2. 判斷父類別指標指向的物件是否為Fish類別
3. 使用dynamic_cast把父類別指標轉成子類別指標，因為父類別Animal是沒有swim()函式。
4. 子類別指標呼叫swim()函式。
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
class Animal{
 public:
  virtual void eat(){}
};
class Wolf: public Animal{
 public:
  void eat(){
    cout << "Wolf eat!" << endl;
  }
};
class Fish: public Animal{
 public:
  void eat(){
    cout << "Fish eat!" << endl;
  }
  void swim() {
    cout << "Fish swim!" << endl;
  }
};
class Bird: public Animal{
 public:
  void eat(){
    cout << "Bird eat!" << endl;
  }
};
class Human: public Animal{
 public:
  void eat(){
    cout << "Human eat!" << endl;
  }
};
class Cat: public Animal{
 public:
  void eat(){
    cout << "Cat eat!" << endl;
  }
};
int main() {
  vector<Animal*> animals;
  animals.push_back(new Wolf());
  animals.push_back(new Fish());
  animals.push_back(new Bird());
  animals.push_back(new Human());
  animals.push_back(new Cat());
  //使用iterator
  vector<Animal*>::iterator it = animals.begin();
  //animals.end()指向最後一個元素的下一個元素
  for(;it != animals.end(); it++) {
    Animal* parent_ptr = *it;
    parent_ptr->eat() ;
    if(typeid(*parent_ptr) == typeid(Fish)) {
      //Fish* fish_ptr = dynamic_cast<Fish*>(parent_ptr);
      Fish* fish_ptr = (Fish*)parent_ptr;
      // 若轉型成功才執行以下程式碼，轉型失敗會傳回nullptr
      if (fish_ptr != nullptr)
        fish_ptr->swim();
    }
  }
  return 0;
}
{% endhighlight %}
```
Wolf eat!
Fish eat!
Fish swim!
Bird eat!
Human eat!
Cat eat!
```

[1]: {% link _pages/c/stl/iterator.md %}#正向疊代器