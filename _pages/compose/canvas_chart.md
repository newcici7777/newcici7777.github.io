---
title: 畫半圓Canvas chart
date: 2023-05-26
keywords: Android, Jetpack compose, Canvas chart
---
## 二個點連成一條線
![img]({{site.imgurl}}/compose/canvas1.png)
0度與180度的二個點連成一條線
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    //0度是水平面 往下畫180半圓
    startAngle = 0f,
    sweepAngle = 180f,
    useCenter = true, //0度與180度的二個點連成一條線
    style = Stroke(30f) // 邊框
  )
}
{% endhighlight %}

## 往下畫180半圓 

![img]({{site.imgurl}}/compose/canvas2.png)
0度是水平面 往下畫180半圓
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    //0度是水平面 往下畫180半圓
    startAngle = 0f,
    sweepAngle = 180f,
    useCenter = false, //圓點之間連起來
    style = Stroke(30f) // 邊框
  )
}
{% endhighlight %}

## 沒設置邊框
![img]({{site.imgurl}}/compose/canvas3.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    //0度是水平面 往下畫180半圓
    startAngle = 0f,
    sweepAngle = 180f,
    useCenter = false, //圓點之間連起來
    //style = Stroke(30f) // 邊框
  )
}
{% endhighlight %}

## 產生邊框圓角
![img]({{site.imgurl}}/compose/canvas4.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    //0度是水平面 往下畫180半圓
    startAngle = 0f,
    sweepAngle = 180f,
    useCenter = false, //圓點之間連起來
    style = Stroke(30f,cap = StrokeCap.Round) // 邊框圓角
  )
}
{% endhighlight %}

## 從-10度到180度，框住的部分就是-10度
![img]({{site.imgurl}}/compose/canvas5.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    //0度是水平面 往下畫180半圓
    startAngle = -10f,
    sweepAngle = 180f,
    useCenter = false, //圓點之間連起來
    style = Stroke(30f,cap = StrokeCap.Round) // 邊框
  )
}
{% endhighlight %}

## 0到200度，從180度至200度為框出來的部分
![img]({{site.imgurl}}/compose/canvas6.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    //0度是水平面 往下畫180半圓
    startAngle = 0f,
    sweepAngle = 200f,
    useCenter = false, //圓點之間連起來
    style = Stroke(30f,cap = StrokeCap.Round) // 邊框
  )
}
{% endhighlight %}

## -10度到200度，把1000point的字差不多高，而且二邊端點是平的
![img]({{site.imgurl}}/compose/canvas7.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    //0度是水平面 往下畫180半圓
    startAngle = -10f,
    sweepAngle = 200f,
    useCenter = false, //圓點之間連起來
    style = Stroke(30f,cap = StrokeCap.Round) // 邊框
  )
}
{% endhighlight %}

## 把下面的半圓整個翻轉變成上面半圓
![img]({{site.imgurl}}/compose/canvas8.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)) {
    //翻轉180度
    rotate(180f) {
      drawArc(
        Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
        //0度是水平面 往下畫180半圓
        startAngle = -10f,
        sweepAngle = 200f,
        useCenter = false, //圓點之間連起來
        style = Stroke(30f, cap = StrokeCap.Round) // 邊框
      )
    }
  }
{% endhighlight %}

## 去掉rotate(180f) {}  從起點160度開始畫200度的弧形 
![img]({{site.imgurl}}/compose/canvas9.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    startAngle = 160f,
    sweepAngle = 200f,
    useCenter = false, //圓點之間連起來
    style = Stroke(30f, cap = StrokeCap.Round) // 邊框
  )

}
{% endhighlight %}

## 把終點設成220，剛好成一個半圓
![img]({{site.imgurl}}/compose/canvas10.png)
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)){
drawArc(
Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
    startAngle = 160f,
    sweepAngle = 220f,
    useCenter = false, //圓點之間連起來
    style = Stroke(30f, cap = StrokeCap.Round) // 邊框
  )

}
{% endhighlight %}

## 再疊上一個白色，起點位置相同，蓋上2/3的部分
![img]({{site.imgurl}}/compose/canvas11.png)
原本200 一半是100 2/3是133  
sweepAngle = 200f  
sweepAngle = 133f  
{% highlight kotlin linenos %}
Canvas(modifier = Modifier.size(boxWidthDp.dp)) {
    //方式一
    //翻轉180度
    rotate(180f) {
      drawArc(
        Color(0, 0, 0, 33),//黑色rgb都0，%3透明度設33
        startAngle = -10f,
        sweepAngle = 200f,
        useCenter = false, //圓點之間連起來
        style = Stroke(30f, cap = StrokeCap.Round) // 邊框
      )
    }

    //覆寫同樣的位置就可以部分蓋住
    rotate(180f) {
      drawArc(
        Color.White,
        startAngle = -10f,
        sweepAngle = 133f,//原本200 一半是100 2/3是133
        useCenter = false, //圓點之間連起來
        style = Stroke(30f, cap = StrokeCap.Round) // 邊框
      )
    }
    
  }
{% endhighlight %}

