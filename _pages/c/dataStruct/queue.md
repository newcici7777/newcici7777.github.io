---
title: Queue實作
date: 2024-09-26
keywords: c++, Queue 
---
{% highlight c++ linenos %}
using namespace std;
typedef int ElemType;
//定義節點
struct Node {
    ElemType data;
    Node *next;
};
struct queue {
    Node *head,*tail;
};
/**
 初始化Queue
 */
bool initQueue(queue& que) {
    //頭節點
    que.head = new (std::nothrow)Node;
    //頭節點記憶體分配失敗
    if(que.head == nullptr) return false;
    //前面節點與後面節點全設為nullptr
    que.head->next = nullptr;//queue的下一個節點為null
    que.tail = que.head;//一開始頭節點與尾節點指向同一個位置
    return true;
}
/**
 摧毀Queue
 參數為頭節點
 */
void destroyQueue(queue& que){
    //暫存節點
    Node* temp;
    while(que.head != nullptr) {
        //先暫存下一個節點
        temp = que.head->next;
        //刪掉目前頭節點
        delete  que.head;
        //頭節點換成下一個節點
        que.head = temp;
    }
}
bool inQueue(queue& que, const ElemType& ee) {
    if(que.head == nullptr) {
        cout << "鏈結串列不存在" << endl;
        return false;
    }
    //建立新節點
    Node* temp = new (std::nothrow)Node;
    if(temp == nullptr) return false;
    
    //新節點設定傳進來的資料
    temp->data = ee;
    temp->next = nullptr;
    
    //將新節點插入到tail之後
    que.tail->next = temp;
    //把tail指向temp
    que.tail = temp;
    return true;
}
bool deQueue(queue& que, ElemType& e) {
    if(que.head == nullptr) {
        cout << "鏈結串列不存在" << endl;
        return false;
    }
    //只有頭節點，沒有後面的資料
    if(que.head->next == nullptr) {
        cout << "沒有任何節點" << endl;
        return false;
    }
    //鏈結串列中第一個節點，不包含頭節點
    Node* first = que.head->next;
    //將第一個節點的值存在e，作為返回
    e = first->data;
    
    //將第一個節點(不包含頭節點)指向first的下個節點
    que.head->next = first->next;
    
    //如果刪的節點是tail節點
    if(first == que.tail)
        //將tail指向head
        que.tail = que.head;
    //將第一個節點刪除
    delete first;
    return true;
}
void printQueue(queue& que) {
    if(que.head == nullptr) {
        cout << "鏈結串列不存在" << endl;
        return;
    }
    //從head的下一個節點開始
    //head不存在值
    Node *p = que.head->next;
    while(p != nullptr) {
        cout << p->data << ",";
        //節點p換成下一個節點
        p = p->next;
    }
    cout << endl;
}
size_t size(queue& que) {
    if(que.head == nullptr) {
        cout << "鏈結串列不存在" << endl;
        return 0;
    }
    //不包含頭節點
    Node* p = que.head->next;
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
/**
 清空鏈表，除了頭節點
 */
void clear(queue& que) {
    if(que.head == nullptr) {
        cout << "鏈結串列不存在" << endl;
        return;
    }
    //移到頭節點的下一個節點(清空不包含頭節點)
    Node* tmp = que.head->next,*tmpnext;
    //繞行所有節點
    while(tmp != nullptr) {
        //把節點的下一個節點暫存
        tmpnext = tmp->next;
        //刪除目前節點
        delete  tmp;
        //將下一個節點作為目前節點
        tmp = tmpnext;
    }
    //頭節點的下一個設null
    que.head->next = nullptr;
    //尾節點與頭節點指向相同
    que.tail = que.head;
}
int main() {
    //建立queue
    queue que;
    //清空
    memset(&que, 0, sizeof(que));
    //初始化
    initQueue(que);
    inQueue(que, 1);
    inQueue(que, 2);
    inQueue(que, 3);
    inQueue(que, 4);
    inQueue(que, 5);
    
    cout << "大小:" << size(que) << endl;
    cout << "queue的內容:" << endl;
    printQueue(que);
    
    ElemType e;
    while(deQueue(que, e))
        cout << e << endl;
    
    inQueue(que, 6);
    inQueue(que, 7);
    inQueue(que, 8);
    inQueue(que, 9);
    inQueue(que, 10);
    
    cout << "大小:" << size(que) << endl;
    cout << "queue的內容:" << endl;
    printQueue(que);
    destroyQueue(que);
    return 0;
}
{% endhighlight %}