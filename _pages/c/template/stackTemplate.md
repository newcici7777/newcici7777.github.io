---
title: Stack實作(int Array)
date: 2024-04-23
keywords: c++, stack
prePage: 
nextPage: 
---
## Stack int陣列實作。
{% highlight c++ linenos %}
class Stack{
private:
    int *items; //陣列指標
    int stacksize;//stack大小
    int top;//目前stack的數量(stack 頂端)
public:
    //建構式，初始化成員stacksize與top
    Stack(int size):stacksize(size),top(0) {
        //建立int陣列指標
        items = new int[stacksize];
    }
    ~Stack() {
        delete [] items;//刪除陣列指標
        items = nullptr;//指向null
    }
    /**
     判斷是否為空
     */
    bool isEmpty() const {
        return top == 0;
    }
    /**
     判斷是否已滿
     */
    bool isfull() {
        return top == stacksize;
    }
    /**
     push stack
     */
    bool push(const int& item) {
        //如果stack數量 小於 stacksize
        if(top < stacksize) {
            items[top++] = item;//把item加入
            return true;
        }
        return false;
    }
    /**
     pop stack
     */
    bool pop(int& item) {
        if(top > 0) {
            item = items[--top];
            return true;
        }
        return false;
    }
};
{% endhighlight %}
第3行，私有成員屬性items，資料型態為int指標，指向int陣列的第0筆位址。  

第4行，私有成員屬性stacksize，資料型態為int，存放stack最大的容量。  

第5行，私有成員屬性top，資料型態為int，記錄目前stack最頂端的index位置。 

第8行，建構式，初始化成員stacksize與top。  

第10行，動態配置int陣列。  

第12行，解構式。  

第13行，從記憶體回收items的空間。  

第14行，將回收的空間指向null。  

第20行，若top位置在0，表示為空。  

第25行，若top位置在stacksize代表空間已滿。  

第31行，函式是將item推入stack，參數為參考資料型態。  

第33行，目前stack最頂端的index位置小於stack最大容量就做if的區塊。  

第34行，先做items[top] = item的動作，再做top++的動作。  

第42行，參數為參考資料型態，stack彈出頂端的值暫時放置的變數。  

第44行，先將top--的動作，再做items[top]的動作。 

### 使用Stack
{% highlight c++ linenos %}
int main() {
    Stack myStack(5);
    myStack.push(1);
    myStack.push(2);
    myStack.push(3);
    myStack.push(4);
    myStack.push(5);
    int item;
    while(myStack.isEmpty() == false) {
        myStack.pop(item);
        cout << "item = " << item << endl;
    }
        return 0;
}
{% endhighlight %}

第2行，建立stack物件，最大存放數量為5。  

第3~7行，依序塞入1~5。  

第8行，宣告暫時存放變數item。  

第9行，若stack不為空就進入循環。  

第10行，將stack的頂端拿出來，放在變數item。  

第11行，印出item。  
```
執行結果 
item = 5
item = 4
item = 3
item = 2
item = 1 
```
## Stack typedef實作

將上面的程式修改成自定義資料型態DataType。以下有黃色的部分是有變更的程式碼。  

<pre>
<span class="markline">typedef int DataType;</span>
class Stack{
private:
    <span class="markline">DataType</span> *items; //陣列指標
    int stacksize;//stack大小
    int top;//目前stack的數量(stack 頂端)
public:
    //建構式，初始化成員stacksize與top
    Stack(int size):stacksize(size),top(0) {
        //建立int陣列指標
        items = new <span class="markline">DataType</span>[stacksize];
    }
    ~Stack() {
        delete [] items;//刪除陣列指標
        items = nullptr;//指向null
    }
    /**
     判斷是否為空
     */
    bool isEmpty() const {
        return top == 0;
    }
    /**
     判斷是否已滿
     */
    bool isfull() {
        return top == stacksize;
    }
    /**
     push stack
     */
    bool push(const <span class="markline">DataType&</span> item) {
        //如果stack數量 小於 stacksize
        if(top < stacksize) {
            items[top++] = item;//把item加入
            return true;
        }
        return false;
    }
    /**
     pop stack
     */
    bool pop(<span class="markline">DataType&</span> item) {
        if(top > 0) {
            item = items[--top];
            return true;
        }
        return false;
    }
};
int main() {
    Stack myStack(5);
    myStack.push(1);
    myStack.push(2);
    myStack.push(3);
    myStack.push(4);
    myStack.push(5);
    <span class="markline">DataType</span> item;
    while(myStack.isEmpty() == false) {
        myStack.pop(item);
        cout << "item = " << item << endl;
    }
    return 0;
}    
</pre>

## Stack 模板實作

將上面的程式再修改成模板，以下有黃色的部分是有變更的程式碼。  

<pre>
<span class="markline">template &lt;class DataType&gt;</span>
class Stack{
private:
    DataType *items; //陣列指標
    int stacksize;//stack大小
    int top;//目前stack的數量(stack 頂端)
public:
    //建構式，初始化成員stacksize與top
    Stack(int size):stacksize(size),top(0) {
        //建立int陣列指標
        items = new DataType[stacksize];
    }
    ~Stack() {
        delete [] items;//刪除陣列指標
        items = nullptr;//指向null
    }
    /**
     判斷是否為空
     */
    bool isEmpty() const {
        return top == 0;
    }
    /**
     判斷是否已滿
     */
    bool isfull() {
        return top == stacksize;
    }
    /**
     push stack
     */
    bool push(const DataType& item) {
        //如果stack數量 小於 stacksize
        if(top < stacksize) {
            items[top++] = item;//把item加入
            return true;
        }
        return false;
    }
    /**
     pop stack
     */
    bool pop(DataType& item) {
        if(top > 0) {
            item = items[--top];
            return true;
        }
        return false;
    }
};
int main() {
    Stack<span class="markline">&lt;int&gt;</span> myStack(5);
    myStack.push(1);
    myStack.push(2);
    myStack.push(3);
    myStack.push(4);
    myStack.push(5);
    <span class="markline">int</span> item;
    while(myStack.isEmpty() == false) {
        myStack.pop(item);
        cout << "item = " << item << endl;
    }
    return 0;
}    
</pre>