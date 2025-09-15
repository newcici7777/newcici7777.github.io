---
title: bool
date: 2025-09-12
keywords: c++, c,
---
# bool

c沒有bool的類型

include

```
#include <stdbool.h>
```

非0為true

{% highlight c++ linenos %}
    if(1) {
        printf("true\n");
    }
{% endhighlight %}

其它類型有值，也為true

```cpp
    int i = 10;
    if(i) {
        printf("true\n");
    }
```
