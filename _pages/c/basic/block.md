---
title: 程式碼區塊或函式主體(Body)
date: 2024-04-18
keywords: c++, printf
---

一對花括號`{}`包起來的就是程式碼區塊或函式主體(Body)。程式碼區塊可以是if(){}或while(){}的程式碼區塊，但這篇要探討的是只有花括號`{}`包起來的程式碼區塊。

### 區域變數
{% highlight c++ linenos %}
int main() {
    int var11 = 100;
    {
        int var11 = 10;
        cout << "inner var11 = " << var11 << endl;
    }
    cout << "outer var11 = " << var11 << endl;
    return 0;
}
{% endhighlight %}
```
執行結果
inner var11 = 10
outer var11 = 100
```
在程式碼區塊中`{}`，所定義的變數都是區域變數，離開`{}`區塊就不能再讀取了，因為`{}`區塊中的區域變數已經被系統回收掉。可以想像`{}`就像是函式一樣。在函式中，函式中定義的變數都是區域變數，離開函式就不能被外部讀取區域變數，因為區域變數已經被系統回收了。

### 外部變數
{% highlight c++ linenos %}
int main() {
    int var11 = 100;
    int var12 = 200;
    {
        int var11 = 10;
        cout << "inner var11 = " << var11 << endl;
        cout << "inner var12 = " << var12 << endl;
    }
    cout << "outer var11 = " << var11 << endl;
    return 0;
}
{% endhighlight %}
```
執行結果
inner var11 = 10
inner var12 = 200
outer var11 = 100
```
注意第7行，在`{}`區塊中，可以讀取外部變數。

### 在程式區塊中`{}`無法讀取同名的外部變數
因為在`{}`區塊中，同名的外部變數會先被暫時隱藏，直到程式執行時離開`{}`區塊，區塊內的區域變數被系統回收掉，同名的變數就會將記憶體位置指向外部變數。

### 程式區塊中`{}`讀取跟區域變數同名的全域變數

使用::可以讀取全域變數。

{% highlight c++ linenos %}
int var11 = 100;
int main() {
    int var12 = 200;
    {
        int var11 = 10;
        cout << "inner var11 = " << var11 << endl;
        cout << "inner var12 = " << var12 << endl;
        cout << "outer var11 = " << ::var11 << endl;
    }
        return 0;
}
{% endhighlight %}

第1行，定義全域變數var11。

第5行，定義區域變數var11。

第6行，印出區域變數var11。

第8行，印出全域變數var11。

```
執行結果
inner var11 = 10
inner var12 = 200
outer var11 = 100
```

### for程式區塊中的區域變數

變數i的有效範圍，只有在`{}`程式區塊中，離開`{}`程式區塊，區域變數i就無法在外部讀取。

{% highlight c++ linenos %}
for(int i = 0; i < 10; i++) {
    
}
{% endhighlight %}

若要變數i也可以在外部使用，可把變數i放在外部。

{% highlight c++ linenos %}
int i = 0;
for(; i < 10; i++) {    
}
cout << "i = " << i << endl;
{% endhighlight %}

```
執行結果
i = 10
```

### 虛擬碼

可以在程式區塊寫虛擬碼，進行驗證，作為之後正式的函式使用。
以下程式碼是在寫參數為指標，將指標宣告記憶體空間。

{% highlight c++ linenos %}
int main() {
    //宣告指標
    int* p = 0;//0就是nullptr 代表沒有指向任何記憶空間
    {
        int** pp = &p;//存放指標的位址，要用雙指標
        *pp = new int(3);//雙指標取值，並動態配置記憶體空間
        
    }
    cout << "p=" << p << ",*p=" << *p << endl;
    return 0;
}
{% endhighlight %}

第3行，宣告指標。

第5行，宣告雙指標，存放指標的位址。

第6行，去雙指標pp的記憶體位址取值，取出來是p的指標記憶體位址，使用new來分配動態配置記憶體空間，並且設值"3"，動態配置完成會回傳記憶體位址，由指標去存位址。

第8行，印出p指標的位址，去p指標的記憶體位址取值，並印出來。

```
執行結果
p=0x60000000c000,*p=3
```
虛擬碼執行成功後，將`{}`程式碼區塊移到main()函式之外，並宣告成函式。
 
{% highlight c++ linenos %}
void initMemory(int** pp) //存放指標的位址，要用雙指標
{
    *pp = new int(3);//雙指標取值，並動態配置記憶體空間
}
int main() {
    //宣告指標
    int* p = 0;//0就是nullptr 代表沒有指向任何記憶空間
    initMemory(&p);//指標的位址傳入函式。
    cout << "p=" << p << ",*p=" << *p << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
p=0x600000008030,*p=3
```