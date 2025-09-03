---
title: scanf
date: 2025-09-03
keywords: c, scanf
---
## scanf格式化
```
scanf("格式化", &記憶體位址);
scanf("%s", &name);
```

|基本型態|格式化|
|char name[]|%s|
|int|%d|
|double|%lf|
|char| %c|

注意！要取得char字元，要scanf2次才取得到。
{% highlight c++ linenos %}
  scanf("%c",&gender);
  scanf("%c", &gender);
{% endhighlight %}

{% highlight c++ linenos %}
int main() {
  char name[10] = "";
  int age = 0;
  double salary = 0.0;
  char gender = ' ';
  printf("請輸入名字");
  scanf("%s", &name);

  printf("請輸入年齡");
  scanf("%d", &age);
  
  printf("請輸入薪水");
  scanf("%lf", &salary);
  
  // F是女 M是男
  printf("請輸入性別:F 或 M");
  scanf("%c",&gender);
  scanf("%c", &gender);
  
  printf("name= %s age= %d sal= %.2f gender = %c \n",name ,age ,salary ,gender);
  return 0;
}
{% endhighlight %}
```
請輸入名字test
請輸入年齡20
請輸入薪水15.5
請輸入性別:F
name= test age= 20 sal= 15.50 gender = F 
```