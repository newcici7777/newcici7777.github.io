---
title: Stack實作
date: 2024-09-25
keywords: c++, Stack 
---

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
bool push(Node* head, const ElemType& ee) {
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
bool pop(Node* head, ElemType& e) {
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
    e = p->data;
    //把p刪掉
    delete p;
    return true;
}
int main() {
    //建立頭節點
    Node* stack = initLinkedList();
    //增加節點
    push(stack, 1);
    //增加節點
    push(stack, 2);
    //增加節點
    push(stack, 3);
    //增加節點
    push(stack, 4);
    //印出所有節點
    printList(stack);
    
    ElemType e;
    pop(stack,e);
    cout << "pop 的值 = " << e << endl;
    //釋放記憶體(包含頭節點)
    destroyLinkedList(stack);
    return 0;
}
{% endhighlight %}