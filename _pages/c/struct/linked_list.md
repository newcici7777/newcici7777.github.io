---
title: 鏈結串列
date: 2024-07-17
keywords: c++, pointer within a Structure, Linked list
---

Prerequisites:

- [結構中的指標是自己][1]

## 初始化頭節點/尾節點/暫存節點

{% highlight c++ linenos %}
    //頭節點
    Student *head = nullptr;
    //尾節點
    Student *tail = nullptr;
    //暫存節點
    Student *temp = nullptr;
{% endhighlight %}

## 建立節點

### 建立第一個節點

{% highlight c++ linenos %}
    //建立第1個節點
    temp = new Student({ "Mary", 1, nullptr});

    //讓頭節點與尾節點全指向新分配的節點
    head = tail = temp;
{% endhighlight %}

### 建立第二個節點

{% highlight c++ linenos %}
    //建立第2個節點
    temp = new Student({ "Bill", 2, nullptr});
    //將上一個節點tail的next指向新建立的節點
    tail->next = temp;
    //將tail指向新建立的節點;
    tail = temp;
{% endhighlight %}

### 建立第三個節點

程式碼與前一個程式一樣。

{% highlight c++ linenos %}
    //建立第3個節點
    temp = new Student({ "Eric", 3, nullptr});
    //將上一個節點tail的next指向新建立的節點
    tail->next = temp;
    //將tail指向新建立的節點;
    tail = temp;
{% endhighlight %}

## 遍歷鏈節串列
從頭節點開始，沿著next指標往後移動，直到next是nullptr。
{% highlight c++ linenos %}
    //遍歷鏈節串列
    //從頭節點開始
    //把頭節點指向temp指標
    temp = head;
    //如果temp指標不是nullptr，就進去while
    while(temp != nullptr) {
        //印出姓名學號
        cout << "id = " << temp->id;
        cout << ", name = " << temp->name << endl;
        //把temp指標移到下一個節點
        temp = temp->next;
    }
{% endhighlight %}

## 刪除鏈節串列
從頭節點開始刪，沿著next指標往後移動，直到next是nullptr。
{% highlight c++ linenos %}
    //刪除鏈節串列
    //從頭節點開始刪除
    //如果頭節點不為nullptr
    while(head != nullptr) {
        //頭節點先暫存到暫存節點
        temp = head;
        //頭節點指標往後移動
        head = head->next;
        //刪除暫存節點
        delete temp;
    }
{% endhighlight %}

## 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
struct Student{
    //學生姓名
    char name[100];
    //學號
    int id;
    //結構中的指標
    Student *next;
};
int main() {
    //頭節點
    Student *head = nullptr;
    //尾節點
    Student *tail = nullptr;
    //暫存節點
    Student *temp = nullptr;
    
    //建立第1個節點
    temp = new Student({ "Mary", 1, nullptr});
    
    //讓頭節點與尾節點全指向新分配的節點
    head = tail = temp;
    
    //建立第2個節點
    temp = new Student({ "Bill", 2, nullptr});
    //將上一個節點tail的next指向新建立的節點
    tail->next = temp;
    //將tail指向新建立的節點;
    tail = temp;
    
    //建立第3個節點
    temp = new Student({ "Eric", 3, nullptr});
    //將上一個節點tail的next指向新建立的節點
    tail->next = temp;
    //將tail指向新建立的節點;
    tail = temp;
    
    //建立第4個節點
    temp = new Student({ "Jeff", 4, nullptr});
    //將上一個節點tail的next指向新建立的節點
    tail->next = temp;
    //將tail指向新建立的節點;
    tail = temp;
    
    //遍歷鏈節串列
    //從頭節點開始
    //把頭節點指向temp指標
    temp = head;
    //如果temp指標不是nullptr，就進去while
    while(temp != nullptr) {
        //印出姓名學號
        cout << "id = " << temp->id;
        cout << ", name = " << temp->name << endl;
        //把temp指標移到下一個節點
        temp = temp->next;
    }
    
    //刪除鏈節串列
    //從頭節點開始刪除
    //如果頭節點不為nullptr
    while(head != nullptr) {
        //頭節點先暫存到暫存節點
        temp = head;
        //頭節點指標往後移動
        head = head->next;
        //刪除暫存節點
        delete temp;
    }
    return 0;
}
{% endhighlight %}
```
id = 1, name = Mary
id = 2, name = Bill
id = 3, name = Eric
id = 4, name = Jeff
```

[1]: {% link _pages/c/struct/pointerInStruct.md %}#結構中的指標是自己