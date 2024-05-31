---
title: 函式參數為指標
date: 2024-05-28
keywords: c++, pointer
---

## 引數與參數

### 引數 Argument

傳給函式的變數。

{% highlight c++ linenos %}
int main() {
    int var1 = 10;
    func1(var1);
    return 0;
}
{% endhighlight %}

第3行，將var1傳到func1()函式中，而var1就是引數Argument。

台灣稱Argument為引數。

大陸稱Argument為實參。

### 參數 Parameter

函式接收到的變數。

{% highlight c++ linenos %}
void func1(int param1) {
    cout << "param1=" << param1 << endl;
}
{% endhighlight %}

第1行，函式接收到來自外面傳進來的變數，變數名param1，而param1就是參數。

台灣稱Parameter為參數。

大陸稱Parameter為形參。

## 修改傳進來的參數

### 指標參數

函式的參數是指標，代表參數是地址。指標參數語法為`類型* 參數名`

```
void func1(int* param1) {
}
```

### 取值運算子，取出指標參數的內容。

當函式的參數為指標，指標就是地址，代表可以透過取值運算子*,取出地址存放的內容。

```
*param1
```
{% highlight c++ linenos %}
void func1(int* param1) {
    cout << "param1=" << *param1 << endl;
}
{% endhighlight %}


```
執行結果
param1=10

```

### 修改指標(地址)的內容

使用`*指標 = 修改內容`，修改指標的內容。

```
*param1 = 20;
```

完整程式碼

{% highlight c++ linenos %}
void func1(int* param1) {
    cout << "param1=" << *param1 << endl;
    *param1 = 20;
}
int main() {
    int var1 = 10;
    cout << "修改前 var1 =" << var1 << endl;
    func1(&var1);
    cout << "修改後 var1 =" << var1 << endl;
    return 0;
}
{% endhighlight %}

第1行，函式的參數是指標，代表參數是地址。

第2行，透過取值運算子*,取出地址存放的內容。

第3行，使用`*指標 = 修改內容`，修改指標的內容。

第8行，使用取地址運算子&，取出var1變數的記憶體開始地址，將地址作為引數Argument傳進函式fun1()。

```
執行結果
修改前 var1 =10
param1=10
修改後 var1 =20
```

### 引數為指標變數

上一個程式是把地址&var1傳進函式，下面的程式碼是先將地址&var放入int*指標變數，再把指標變數傳入函式。

{% highlight c++ linenos %}
void func1(int* param1) {
    cout << "param1=" << *param1 << endl;
}
int main() {
    int var1 = 10;
    int* p = &var1;
    func1(p);
    return 0;
}
{% endhighlight %}

```
執行結果
param1=10
```

## 作為函式返回值

如果有一個功能要求函式返回多個值，只有一個函式返回值根本不夠用，可以把參數作為返回值。

以下程式是在一群學生數學/英文/歷史成績中，分別找出各科數學/英文/歷史最大的分數。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
class Student {
public:
    Student(string name, double mathScore, double engScore, double historyScore)
    :name(name),mathScore(mathScore),engScore(engScore),historyScore(historyScore){}
    string getName(){
        return name;
    }
    double getMathScore(){
        return mathScore;
    }
    double getEngScore(){
        return engScore;
    }
    double getHistoryScore(){
        return historyScore;
    }
private:
    string name;
    double mathScore;
    double engScore;
    double historyScore;
};

void getMax(Student studentArr[], int arrSize, double* mathMax, double* engMax, double* historyMax){
    for(int i = 0; i < arrSize; i++) {
        if(studentArr[i].getMathScore() > *mathMax)
            *mathMax = studentArr[i].getMathScore();
        if(studentArr[i].getEngScore() > *engMax)
            *engMax = studentArr[i].getEngScore();
        if(studentArr[i].getHistoryScore() > *historyMax)
            *historyMax = studentArr[i].getHistoryScore();
    }
}

int main() {
    Student studentArr[] =
    {Student("Mary", 50, 30, 100),
     Student("Bill", 70, 80, 90),
     Student("Lisa", 20, 99, 10)};
    double* mathMax = new double(0);
    double* engMax = new double(0);
    double* historyMax = new double(0);
    compare(studentArr, 3, mathMax, engMax, historyMax);
    cout << "mathMax = " << *mathMax << endl;
    cout << "mathEng = " << *engMax << endl;
    cout << "mathHistory = " << *historyMax << endl;
    return 0;
}
{% endhighlight %}

第2行，宣告Student類別。

第4行，Student建構式，將參數設給私有成員name, mathScore, engScore, historyScore。

第7,10,13,16行，取出name, mathScore, engScore, historyScore公有方法。

第20,21,22,23行，私有成員屬性name, mathScore, engScore, historyScore。

第26行，宣告函式，參數為Student陣列，陣列大小，maxMax指標,engMax指標,historyMax指標

第29,31,33行，找出數學/英文/歷史最大的分數。

第39行，建立3個學生物件，利用名字,數學英文歷史成績建立物件。

第44,45,46行，宣告三個存最大數學/英文/歷史成績的指標。

第47,將Student陣列，陣列大小，最大數學/英文/歷史成績的指標傳入getMax函式。

第48,49,50行，印出各科最大分數。

```
執行結果
mathMax = 70
mathEng = 99
mathHistory = 100
```

## 減少記憶體空間的使用

每呼叫一次函式，就會為函式參數分配記憶體空間，使用指標作為參數，指標占記憶體空間固定8 byte，若參數為char[100]則會占100 byte的記憶體空間，相比之下指標可以節省更多記憶體空間。