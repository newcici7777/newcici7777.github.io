---
title: Enum列舉
date: 2024-10-15
keywords: c++, enum 
---
列舉型態在記憶體中儲存為整數。
## enum宣告
### 語法1
```
enum 列舉類型名{常數名 = 值, 常數名 = 值, 常數名 = 值, ...};
列舉類型名 變數;
```
{% highlight c++ linenos %}
enum DAY {
  MON = 1, TUE = 2, WED = 3, THU = 4, FRI = 5, SAT = 6, SUN = 7
};
DAY day;
{% endhighlight %}

### 語法2
```
enum 列舉類型名{常數名 = 值, 常數名 = 值, 常數名 = 值, ...}變數;
```
{% highlight c++ linenos %}
enum DAY {
  MON = 1, TUE = 2, WED = 3, THU = 4, FRI = 5, SAT = 6, SUN = 7
} day;
{% endhighlight %}

## 變數設值
```
列舉變數 = 列舉常數名
```

{% highlight c++ linenos %}
int main() {
  enum DAY {
    MON = 1, TUE = 2, WED = 3, THU = 4, FRI = 5, SAT = 6, SUN = 7
  };
  DAY day;
  day = WED;
  printf("%d",day);
  return 0;
}
{% endhighlight %}
```
3
```

不能直接給數字，以下編譯錯誤。
{% highlight c++ linenos %}
int main() {
  enum DAY {
    MON = 1, TUE = 2, WED = 3, THU = 4, FRI = 5, SAT = 6, SUN = 7
  };
  DAY day;
  day = 1;
  printf("%d",day);
  return 0;
}
{% endhighlight %}

使用強制轉型，可以把數字轉成列舉類型
{% highlight c++ linenos %}
    day = (DAY) i;
{% endhighlight %}

## 預設值從0開始遞增
如果沒有給值，自動從0開始遞增。
{% highlight c++ linenos %}
int main() {
  enum DAY {
    MON, TUE, WED, THU, FRI, SAT, SUN
  };
  DAY day;
  for(int i = MON; i <= SUN; i++) {
    day = (DAY) i;
    printf("%d \n",day);
  }
  return 0;
}
{% endhighlight %}
```
0 
1 
2 
3 
4 
5 
6
```

## 列舉運算
列舉本身是整數。
{% highlight c++ linenos %}
enum DAY {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
int main() {
  DAY day1, day2;
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

