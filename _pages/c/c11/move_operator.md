---
title: 移動建構子與移動指派運算子
date: 2024-11-4
keywords: c++, Move assignment operator 
---

Prerequisites:

- [拷貝函式][1]
- [operator=()][2]
- [深淺拷貝][3]
- [l-value與r-value][4]
- [l-value參考與r-value參考][5]

## 目的

以下的拷貝函式，針對指標(m_ptr)動態分配記憶體位址，將來源(source)物件指標內容拷貝到新分配的記憶體位址。
{% highlight c++ linenos %}
    Student(const Student &s) {
        //判斷來源(source)m_ptr是否null
        if(s.m_ptr) {
        	//動態分配記憶體位址
            m_ptr = new int;
            //拷貝來源物件m_ptr指標到新的記憶體位址
            memcpy(m_ptr, src.m_ptr, sizeof(int));
        } else {
            m_ptr = nullptr;
        }
    }
{% endhighlight %}

以上程式碼若拷貝的容量很大，將會花費許多時間在拷貝資料。

移動建構子(Move constructor)與移動指派運算子(Move assignment operator)主要針對右值進行指標移動，因為右值是一個暫存物件，通常使用完畢立即記憶體釋放，移動建構子與移動指派運算子是不釋放暫存物件記憶體，把指標指向暫存物件(來源物件)的記憶體位址，省略以上程式碼拷貝動作。

## 語法

移動建構子(Move constructor)與移動指派運算子(Move assignment operator)的參數都是右值(r-value)，因為要刪除來源物件指標(有修改的動作)，所以參數不用const，拷貝與指派運算子則是不會修改來源物件。

### 移動建構子(Move constructor)

```
類別名(類別名&& 來源物件) {
	....
}
```

### 移動指派運算子(Move assignment operator)

```
類別名& operator=(類別名&& 來源物件) {
	......
	return *this;
}
```

### 指標轉移語法

{% highlight c++ linenos %}
    Student(Student&& src) {
        cout << "移動建構子" << endl;
        //如果指標不是nullptr，先把指標釋放
        if(m_ptr != nullptr) delete m_ptr;
        //把來源的指標移到m_ptr
        m_ptr = src.m_ptr;
        //把來源的指標設nullptr
        src.m_ptr = nullptr;
    }
    Student& operator=(Student&& src) {
        cout << "移動指派運算子" << endl;
        //如果指標不是nullptr，先把指標釋放
        if(m_ptr != nullptr) delete m_ptr;
        //把來源的指標移到m_ptr
        m_ptr = src.m_ptr;
        //把來源的指標設nullptr
        src.m_ptr = nullptr;
        return *this;
    }
{% endhighlight %}


[1]: {% link _pages/c/class/copy_constructor.md %}
[2]: {% link _pages/c/class/operator_assign.md %}
[3]: {% link _pages/c/class/deep_shallow_copy.md %}
[4]: {% link _pages/c/c11/rvalue/l_r_value.md %}
[5]: {% link _pages/c/c11/rvalue/l_r_ref.md %}