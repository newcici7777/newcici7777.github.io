---
title: time
date: 2024-12-11
keywords: c++, time
---
include語法
```
#include <time.h>
```

## 計算程式執行時間
{% highlight c++ linenos %}
#include <iostream>
#include <time.h>
using namespace std;
void func() {
  int temp;
  for (int i=0 ; i <1000; i++) {
    temp = i;
  }
}
int main() {
  //time_t結構
  time_t start_t, end_t;
  double diff;

  // 將time_t結構記憶體位址傳入
  time(&start_t);
  // 要執行的函式
  func();
  // 將time_t結構記憶體位址傳入
  time(&end_t);
  // 計算差距秒數
  // end_t - start_t
  diff = difftime(end_t, start_t);
  cout << "second = " << diff << endl;
  return 0;
}
{% endhighlight %}
```
second = 0
```

## 取得現在時間
取得從1970/01/01至今累加的秒數，以長整數(long)存放此秒數。

語法
```
time_t time(time_t *tloc);
```
- 參數代入nullptr = 0

{% highlight c++ linenos %}
#include <iostream>
#include <time.h>
using namespace std;
int main() {
  // 方式1
  // 把nullptr也就是0作為參數代入time()函式
  time_t now = time(0);
  cout << "now = " << now << endl;
  // 方式2
  // 將now2的位址作為參數代入time()函式
  time_t now2;
  time(&now2);
  cout << "now2 = " << now2 << endl;
  return 0;
}
{% endhighlight %}
```
now = 1733883906
now2 = 1733883906
```

## tm結構
{% highlight c++ linenos %}
struct tm
{
  int tm_year;  // 年份：其值等於實際年份減去1900
  int tm_mon;   // 月份：取值區間為[0,11]，其中0代表一月，11代表12月
  int tm_mday;  // 日期：一個月中的日期，取值區間為[1,31]
  int tm_hour;  // 時：取值區間為[0,23]
  int tm_min;   // 分：取值區間為[0,59]
  int tm_sec;   // 秒：取值區間為[0,59]
  int tm_wday;  // 星期：取值區間為[0,6]，其中0代表星期日，6代表星期六
  int tm_yday;  // 從每年的1月1日開始算起的天數：取值區間為[0,365]
  int tm_isdst; // 夏令時標識符，此欄位意義不大
};
{% endhighlight %}

## localtime

- [string append][1]
- [to_string()][2]

將time_t整數轉成tm結構

{% highlight c++ linenos %}
#include <iostream>
#include <time.h>
using namespace std;
int main() {
  time_t now = time(0);
  tm tmnow;
  localtime_r(&now, &tmnow);
  string time_str =
      to_string(tmnow.tm_year + 1900) + "/" +
      to_string(tmnow.tm_mon + 1) + "/" +
      to_string(tmnow.tm_mday) + " " +
      to_string(tmnow.tm_hour) + ":" +
      to_string(tmnow.tm_min) + ":" +
      to_string(tmnow.tm_sec);
  cout << "time_str = " << time_str << endl;
  return 0;
}
{% endhighlight %}
```
time_str = 2024/12/11 11:48:45
```

## 計算執行時間

### gettimeofday

include
```
#include <sys/time.h>
```

語法
```
int gettimeofday(struct timeval *tv, struct timezone *tz);
```

用到的參數結構
```
struct timeval {
  time_t tv_sec;  // 1970-01-01開始的秒數
  suseconds_t tv_usec;  // 表示微秒（1秒 = 1000000微秒）
};
```
{% highlight c++ linenos %}
#include <iostream>
#include <sys/time.h>
using namespace std;
int main() {
  timeval start, end;
  // 開始計時
  gettimeofday(&start, nullptr);
  for (int i = 0; i < 1000000; i++);
  // 結束計時
  gettimeofday(&end, nullptr);
  // 儲存耗時的秒數與微秒
  timeval t_val;
  // end微秒與start微秒相減
  t_val.tv_usec = end.tv_usec - start.tv_usec;
  // end秒數與start秒數相減
  t_val.tv_sec = end.tv_sec - start.tv_sec;
  // 若end微秒與start微秒相減為負數
  if (t_val.tv_usec < 0) {
    // 將負的微秒轉正
    // 使用固定公式 tv_usec = 1000000 + tv_usec
    // 例如：若 tv_usec = -200，則變為 tv_usec = 1000000 - 200 = 999800。
    t_val.tv_usec = 1000000 + t_val.tv_usec;
    // 同時，從秒部分（tv_sec）中減去 1，補償這多出的一秒。
    t_val.tv_sec--;
  }
  cout << "耗費:" << t_val.tv_sec << "秒" << t_val.tv_usec << "微秒。" << endl;
//  假設
//  開始時間為 start = {tv_sec = 10, tv_usec = 500000}，
//  結束時間為 end = {tv_sec = 12, tv_usec = 300000}。
//  則：
//  初始計算：
//  tv_usec = 300000 - 500000 = -200000
//  tv_sec = 12 - 10 = 2
//  微秒補正：
//  tv_usec = 1000000 - 200000 = 800000
//  tv_sec = 2 - 1 = 1
//  最終結果：
//  tv_sec = 1
//  tv_usec = 800000
//  表示耗時 1秒800毫秒。
  return 0;
}
{% endhighlight %}
```
耗費:0秒1213微秒。
```

[1]: {% link _pages/c/stl/string.md %}#字串連接
[2]: {% link _pages/c/string/string_convert.md %}