---
title: string 字串
date: 2024-07-22
keywords: c++, string
---

## string陣列

判斷數字，印出月份英文

{% highlight c++ linenos %}
//12個月
const int MAX_MONTH = 12;
int main() {
    string mon_arr[MAX_MONTH] =
    {"January","February","March","April","May","June","July","August","September","October","November","December"};
    int month;
    cout << "請輸入數字月份(1~12):";
    cin >> month;
    //陣列索引介於0..11，所以要把month-1
    cout << mon_arr[month-1] << endl;
    return 0;
}
{% endhighlight %}

```
請輸入數字月份(1~12):1
January
```
