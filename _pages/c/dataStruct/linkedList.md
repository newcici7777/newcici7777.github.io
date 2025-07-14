---
title: 單向鏈結串列實作
date: 2024-09-21
keywords: c++, LinkedList
---

## 頭節點

頭節點是沒有值，頭節點的next指向的是第一個節點，最後一個節點的next指向nullptr

![img]({{site.imgurl}}/dataStruct/linkedList1.jpg)  

## 結構定義

節點有二個欄位，分別是data與next，data是存放值，next是指向下一個節點

{% highlight c++ linenos %}
typedef int ElemType;
//定義節點
struct Node {
  ElemType data;
  Node* next;
};
{% endhighlight %}

## 初始化

初始化判斷頭節點(head)記憶體位址有沒有建立成功

將頭節點(head)的next(下一個節點位址)設為nullptr

返回頭節點的記憶體位址(使用指標傳回)，new動態分配記憶體位址，會傳回位址，需要用指標來接。

在main()中建立頭節點

{% highlight c++ linenos %}
/**
 初始化鏈結串列
 */
Node* initLinkedList() {
  //頭節點
  Node* head = new (std::nothrow)Node;
  //頭節點記憶體分配失敗
  if(head == nullptr) return nullptr;
  head->next = nullptr;//下一個節點是null
  return head;
}
int main() {
  //建立頭節點
  Node* head = initLinkedList();
  return 0;
}
{% endhighlight %}

## 刪除鏈結串列

刪除所有鏈結串列，包含頭節點(head)

參數是頭節點(head)記憶體位址

繞行所有節點，先把下一個節點存到暫存節點，再刪掉頭節點，再把暫存節點設為頭節點，如此反覆進行，直至下一個節點為nullptr設到head，離開迴圈。

{% highlight c++ linenos %}
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
int main() {
  //建立頭節點
  Node* head = initLinkedList();
  destroyLinkedList(head);
  return 0;
}
{% endhighlight %}

## 新增在最前面

有一個資料要新增在最前面

  
![img]({{site.imgurl}}/dataStruct/linkedList2.jpg)

頭節點的next指向新節點，新節點的next指向原本的第一個節點

![img]({{site.imgurl}}/dataStruct/linkedList3.jpg)
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
  //頭節點的下一個是新節點
  head->next = temp;
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
  destroyLinkedList(head);
  return 0;
}  
{% endhighlight %}

## 新增在最後面

要將3的資料新增在最後面

![img]({{site.imgurl}}/dataStruct/linkedList5.jpg)

新增步驟如下

- 先移動到最後一個節點(不是nullptr)
- 建立新節點，將新節點的next設為nullptr
- 尾節點的next設為新節點

![img]({{site.imgurl}}/dataStruct/linkedList6.jpg)

{% highlight c++ linenos %}
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
  //新節點的下一個節點是null
  temp->next = nullptr;
  
  //尾節點的下一個是新節點
  p->next = temp;
  return true;
}
{% endhighlight %}

## 刪除最前面

把頭節點(head)的next指向要刪除節點的下一個節點

![img]({{site.imgurl}}/dataStruct/linkedList7.jpg)

{% highlight c++ linenos %}
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
  //把p刪掉
  delete p;
  return true;
}
{% endhighlight %}

## 刪除最後一個節點


要刪除最後一個節點，要先找到倒數第二個節點。
把倒數第二個節點的next指向nullptr

![img]({{site.imgurl}}/dataStruct/linkedList8.jpg)

{% highlight c++ linenos %}
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
{% endhighlight %}

## 印出所有節點

因為頭節點head是沒有資料，所以一開始是把p指標指向head的下一個節點。

![img]({{site.imgurl}}/dataStruct/linkedList4.jpg)

然後將p指標一直往next移動，直至指到nullptr。

{% highlight c++ linenos %}
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
  destroyLinkedList(head);
  return 0;
}  
{% endhighlight %}

## 新增節點在某個節點之前

Data3要新增Data2前面

![img]({{site.imgurl}}/dataStruct/linkedList9.jpg)

破解傳統的方式，傳統的方式是用迴圈繞行鏈結串列，找到下一個節點是Data2，才把Data3插進去，新的方式如下:

步驟如下

- Data2的值拷貝到新節點Data3
- 新節點Data3的next設為Data2的next
- Data2的next設為Data3
- Data2的值設成新的資料內容

![img]({{site.imgurl}}/dataStruct/linkedList10.jpg)

{% highlight c++ linenos %}
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
  
  //將節點的資料複製到新節點
  new_node->data = node->data;
  new_node->next = node->next;
  
  //把節點的內容改成新的內容
  node->data = e;
  
  //下一個節點指向新節點
  node->next = new_node;
  return true;
}
{% endhighlight %}

## 刪除某個節點

![img]({{site.imgurl}}/dataStruct/linkedList11.jpg)

因為節點不知道上一個節點是那個？
所以採用以下作法

- 把下一個節點的值複製在要刪除的節點
- 把要刪除的節點的next指向下一個節點的next
- 把下一個節點刪除

![img]({{site.imgurl}}/dataStruct/linkedList12.jpg)

但要刪除的節點沒下一個節點的話，使用`刪除最後一個節點`的函式

{% highlight c++ linenos %}
/**
 將節點刪除
 */
bool deleteNode(Node* node) {
  if(node == nullptr) {
    cout << "節點不存在" << endl;
    return false;
  }
  //若節點為最後一個節點
  if(node->next == nullptr) {
    //使用其它函式removeLast()
    cout << "要刪除的節點是最後一個節點，請使用removeLast()" << endl;
    return false;
  }
  //取得後面節點
  Node* after = node->next;
  //將後面的節點資料，複製到要刪除的節點中
  node->data = after->data;
  node->next = after->next;
  
  //把後面的節點刪除
  delete after;
  return true;
}
{% endhighlight %}

## 取得鏈結串列長度

大小不包含頭節點(head)，將鏈結串列繞行一遍就會知道有多少節點。

{% highlight c++ linenos %}
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
{% endhighlight %}

## 取得某個節點

{% highlight c++ linenos %}
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
{% endhighlight %}

## 完整程式碼

{% highlight c++ linenos %}
typedef int ElemType;
//定義節點
struct Node {
  ElemType data;
  Node* next;
};

/**
 初始化鏈結串列
 */
Node* initLinkedList() {
  //頭節點
  Node* head = new (std::nothrow)Node;
  //頭節點記憶體分配失敗
  if(head == nullptr) return nullptr;
  head->next = nullptr;//下一個節點是null
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
  //頭節點的下一個是新節點
  head->next = temp;
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
  
  //將節點的資料複製到新節點
  new_node->data = node->data;
  new_node->next = node->next;
  
  //把節點的內容改成新的內容
  node->data = e;
  
  //下一個節點指向新節點
  node->next = new_node;
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
  //若節點為最後一個節點
  if(node->next == nullptr) {
    //使用其它函式removeLast()
    cout << "要刪除的節點是最後一個節點，請使用removeLast()" << endl;
    return false;
  }
  //取得後面節點
  Node* after = node->next;
  //將後面的節點資料，複製到要刪除的節點中
  node->data = after->data;
  node->next = after->next;
  
  //把後面的節點刪除
  delete after;
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