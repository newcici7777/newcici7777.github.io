---
title: 雙向鏈結串列實作
date: 2024-09-23
keywords: c++, Double LinkedList
---

雙向鏈結串列代表的是結構中有二個指標，分別指向前面節點跟後面節點

![img]({{site.imgurl}}/dataStruct/doubleList1.jpg)  


## 結構定義

{% highlight c++ linenos %}
typedef int ElemType;
//定義節點
struct Node {
  ElemType data;
  Node *prev,*next;//前面節點與後面節點
};
{% endhighlight %}

## 初始化

以下程式碼是指把指向前面節點與後面節點的指標設為nullptr

{% highlight c++ linenos %}
head->prev = head->next = nullptr;
{% endhighlight %}

{% highlight c++ linenos %}
/**
 初始化鏈結串列
 */
Node* initLinkedList() {
  //頭節點
  Node* head = new (std::nothrow)Node;
  //頭節點記憶體分配失敗
  if(head == nullptr) return nullptr;
  //前面節點與後面節點全設為nullptr
  head->prev = head->next = nullptr;
  return head;
}
{% endhighlight %}


## 最前面新增節點

資料3要插入在最前面

![img]({{site.imgurl}}/dataStruct/doubleList2.jpg)  

步驟如下

- 建立新節點
- 新節點的next指向第一個節點
- 新節點的prev指向頭節點
- 頭節點的next指向新節點
- 第一個節點的prev指向新節點(如果沒有第一個節點就不用做這個步驟)

![img]({{site.imgurl}}/dataStruct/doubleList3.jpg)  

{% highlight c++ linenos %}
bool addFirst(Node* head, const ElemType& ee) {
  if(head == nullptr) {
    cout << "鏈結串列不存在" << endl;
    return false;
  }
  //建立新節點
  Node* temp = new (std::nothrow)Node;
  if(temp == nullptr) return false;
  
  //新節點設定傳進來的資料
  temp->data = ee;
  
  //新節點的下一個節點是頭節點的下一個
  temp->next = head->next;
  //前面節點
  temp->prev = head;
  //頭節點的下一個是新節點
  head->next = temp;
  
  //下一個節點的前面節點，指向新建的節點
  if(temp->next != nullptr)
    temp->next->prev = temp;
  return true;
}
{% endhighlight %}

## 刪除節點

![img]({{site.imgurl}}/dataStruct/doubleList4.jpg)  

步驟如下

- 將待刪除節點的前一個節點next指向待刪除節點的下一個節點
- 將待刪除節點的下一個節點的prev指向待刪除節點的前一個節點

{% highlight c++ linenos %}
/**
 將節點刪除
 */
bool deleteNode(Node* node) {
  if(node == nullptr) {
    cout << "節點不存在" << endl;
    return false;
  }
  //node的前一個節點的next指向node的下一個節點
  node->prev->next = node->next;
  if(node->next != nullptr) {
    //node的下一個節點的prve指向node的prev
    node->next->prev = node->prev;
  }
  delete node;
  return true;
}
{% endhighlight %}

## 完整程式碼
{% highlight c++ linenos %}
using namespace std;
typedef int ElemType;
//定義節點
struct Node {
  ElemType data;
  Node *prev,*next;//前面節點與後面節點
};

/**
 初始化鏈結串列
 */
Node* initLinkedList() {
  //頭節點
  Node* head = new (std::nothrow)Node;
  //頭節點記憶體分配失敗
  if(head == nullptr) return nullptr;
  //前面節點與後面節點全設為nullptr
  head->prev = head->next = nullptr;
  return head;
}

/**
 鏈結串列記憶體回收
 參數為頭節點
 */
void destroyLinkedList(Node* head){
  if(head == nullptr) {
    cout << "鏈結串列不存在" << endl;
    return;
  }
  Node *temp; //暫存節點
  //繞行鏈結串列
  while(head != nullptr) {
    //先把下一個節點存起來
    temp = head->next;
    //刪掉目前的節點
    delete head;
    //把剛才存下來的節點作為頭節點
    head = temp;
  }
}

bool addFirst(Node* head, const ElemType& ee) {
  if(head == nullptr) {
    cout << "鏈結串列不存在" << endl;
    return false;
  }
  //建立新節點
  Node* temp = new (std::nothrow)Node;
  if(temp == nullptr) return false;
  
  //新節點設定傳進來的資料
  temp->data = ee;
  
  //新節點的下一個節點是頭節點的下一個
  temp->next = head->next;
  //前面節點
  temp->prev = head;
  //頭節點的下一個是新節點
  head->next = temp;
  
  //下一個節點的前面節點，指向新建的節點
  if(temp->next != nullptr)
    temp->next->prev = temp;
  return true;
}
bool addLast(Node* head, const ElemType& e) {
  if(head == nullptr) {
    cout << "鏈結不存在" << endl;
    return false;
  }
  //先找到尾節點
  Node *p = head;
  //判斷節點的下一個是否是null
  while(p->next != nullptr) {
    p = p->next;
  }
  
  //建立新節點
  Node* temp = new (std::nothrow)Node;
  if(temp == nullptr) return false;
  
  //新節點設定傳進來的資料
  temp->data = e;
  //新節點的前面節點
  temp->prev = p;
  
  //新節點的下一個節點是null
  temp->next = nullptr;
  
  //尾節點的下一個是新節點
  p->next = temp;
  return true;
}

/**
 在某個節點前插入元素
 參數Node* node 某個節點前
 參數ElemType& e 插入元素
 */
bool insert(Node* node, const ElemType& e) {
  if(node == nullptr) {
    cout << "節點不存在" << endl;
    return false;
  }
  //建立新節點
  Node* new_node = new Node;
  //塞入資料
  new_node->data = e;
  //新節點的前一個節點是插入節點的前一個
  new_node->prev = node->prev;
  //插入節點的前個節點的下一個是新節點
  node->prev->next = new_node;
  //插入節點的前一個節點是新節點
  node->prev = new_node;
  return true;
}
/**
 將節點刪除
 */
bool deleteNode(Node* node) {
  if(node == nullptr) {
    cout << "節點不存在" << endl;
    return false;
  }
  //node的前一個節點的next指向node的下一個節點
  node->prev->next = node->next;
  if(node->next != nullptr) {
    //node的下一個節點的prve指向node的prev
    node->next->prev = node->prev;
  }
  delete node;
  return true;
}
Node* getNode(const Node* head, const ElemType& e) {
  Node* p = head->next;//排除頭節點，從第1個節點開始繞
  while(p != nullptr) {
    //如果節點的資料與參數e相同，傳回節點
    if(p->data == e) return p;
    //移到下一個節點
    p = p->next;
  }
  //最後的是nullptr
  //返回nullptr
  return p;
}
void printList(const Node* head) {
  if(head == nullptr) {
    cout << "鏈結串列不存在" << endl;
    return;
  }
  //從head的下一個節點開始
  //head不存在值
  Node *p = head->next;
  while(p != nullptr) {
    cout << p->data << ",";
    //節點p換成下一個節點
    p = p->next;
  }
  cout << endl;
}
size_t size(Node* head) {
  if(head == nullptr) {
    cout << "鏈結串列不存在" << endl;
    return 0;
  }
  //不包含頭節點
  Node* p = head->next;
  size_t size = 0;
  //繞行到null
  while(p != nullptr) {
    p = p->next;
    size++;
  }
  return size;
  //遞迴方式
  //if(head == nullptr) return 0;
  //return size(head->next) + 1;
}
bool removeFirst(Node* head) {
  if(head == nullptr) {
    cout << "鏈結串列不存在" << endl;
    return false;
  }
  //只有頭節點，沒有後面的資料
  if(head->next == nullptr) {
    cout << "沒有任何節點" << endl;
    return false;
  }
  //不包含頭節點，取得頭節點的下一個
  Node* p = head->next;
  //頭節點的下一個是 p的下一個
  head->next = p->next;
  //p的下一個節點的prev是頭節點
  p->next->prev = head;
  //把p刪掉
  delete p;
  return true;
}
/**
 刪除鏈結串列，但頭節點不刪除
 */
void clearList(Node* head) {
  if(head == nullptr) {
    cout << "鏈結串列不存在" << endl;
    return;
  }
  //不包含頭節點，頭節點下一個節點不存在
  if(head->next == nullptr) {
    cout << "鏈結串列沒有節點" << endl;
    return;
  }
  Node* temp1;
  //從下一個節點開始刪，頭節點要保留
  Node* temp2 = head->next;
  while(temp2 != nullptr) {
    //下一個節點先丟到temp1
    temp1 = temp2->next;
    //刪掉temp2
    delete temp2;
    //temp2變成下一個節點
    temp2 = temp1;
  }
  
  head->next = nullptr;
}
/**
 刪除最後一個節點
 參數頭節點
 */
bool removeLast(Node* head) {
  if(head == nullptr) {
    cout << "鏈結不存在" << endl;
    return false;
  }
  
  Node *p = head;
  //判斷是不是至少有一個節點
  if(head->next == nullptr){
    cout << "鏈結沒結點" << endl;
    return false;
  }
  //先找到倒數第二個節點
  while(p->next->next != nullptr) {
    p = p->next;
  }
  //刪掉最後一個節點
  delete p->next;
  //把倒數第二個節點作為最後一個節點
  //下一個節點為null
  p->next = nullptr;
  return true;
}
int main() {
  //建立頭節點
  Node* head = initLinkedList();
  //增加節點
  addFirst(head, 1);
  //增加節點
  addFirst(head, 2);
  //增加節點
  addFirst(head, 3);
  //增加節點
  addFirst(head, 4);
  //印出所有節點
  printList(head);
  
  cout << "--------------" << endl;
  //增加節點
  addLast(head, 5);
  //增加節點
  addLast(head, 6);
  //增加節點
  addLast(head, 7);
  //增加節點
  addLast(head, 8);
  //印出所有節點
  printList(head);
  cout << "size = " << size(head) << endl;
  
  cout << "--------------" << endl;
  //取得資料為"5"的節點
  Node* node = getNode(head, 5);
  //在資料為"5"的節點之前，新增54
  insert(node, 54);
  //印出所有節點
  printList(head);
  //刪除54節點
  deleteNode(getNode(head, 54));
  //印出所有節點
  printList(head);
  
  //刪除第一個節點
  removeFirst(head);
  //印出所有節點
  printList(head);
  //刪除最後一個節點
  removeLast(head);
  //印出所有節點
  printList(head);
  
  //清空所有節點，(不包含頭節點)
  clearList(head);
  
  //釋放記憶體(包含頭節點)
  destroyLinkedList(head);
  return 0;
}
{% endhighlight %}
