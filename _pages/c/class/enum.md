---
title: Enum列舉
date: 2024-10-15
keywords: c++, enum 
---

## 主程式

### enum宣告

語法

```
enum 變數名稱{常數1, 常數2, 常數3, 常數4, ...};
```

列舉型態在內部視為整數，通常集合第一個元素值為0，下一個為1，以此類推。

以下的寫法是錯誤，編譯器編不過。
```
day1 = 5;
```

### 完整程式碼

{% highlight c++ linenos %}
enum days_of_week {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
int main() {
    days_of_week day1, day2;
    day1 = Mon;
    day2 = Thu;
    
    int diff = day2 - day1;
    cout << "Days between = " << diff << endl;
    return 0;
}    
{% endhighlight %}

```
Days between = 3
```

## 類別與Enum列舉

### enum宣告

```
enum {girl = 0, boy = 1};
```

### 變數存放enum

要記得存放enum值的變數

```
    int sex;
```

### 指派enum給變數

在類別之外設定列舉
```
student.sex = student.girl;
```

#### 完整程式碼

{% highlight c++ linenos %}
class Student {
public:
    char name[50];
    int sex;
    enum {girl = 0, boy = 1};
private:
    char address[100];
public:
    int age;
private:
    char father[50];
public:
    void setName(const char* name1) {
        strcpy(name, name1);
    }
    void setAge(const int age) {
        this->age = age;
    }
    void print() {
        cout << "name: " << name << endl;
        cout << "age: " << age << endl;
        cout << "sex: ";
        if(sex == girl)
            cout << "girl" << endl;
        else
            cout << "boy" <<endl;
    }
};
int main() {
    Student student;
    student.setName("Bill");
    student.setAge(20);
    student.sex = student.girl;
    student.print();
    return 0;
}
{% endhighlight %}

