---
title: 輪播
date: 2023-05-03
keywords: Android, Jetpack compose, HorizontalPager
---
```
假設實際只有3筆
但virtualCount虛擬數量設為9
page頁數    0         1        2
虛擬index 0,1,2     3,4,5    6,7,8
實際下標  [0,1,2]   [0,1,2]  [0,1,2]
分為4頁，每一頁的下標分別為真實的數量0,1,2
虛擬index-[第幾頁(虛擬index/實際總筆數)*實際總筆數]= 實際下標
如何取得真正下標的公式假設虛擬index為3-[第幾頁(虛擬index3/實際總筆數3)*實際總筆數3] = 實際下標0
如何取得真正下標的公式假設虛擬index為4-[第幾頁(虛擬index4/實際總筆數3)*實際總筆數3] = 實際下標1
如何取得真正下標的公式假設虛擬index為5-[第幾頁(虛擬index5/實際總筆數3)*實際總筆數3] = 實際下標2

但現在有一個問題，初始值index，假設虛擬總筆數為8 初始值為8/2 = 4
4為虛擬初始值，套上面的公式 4-[(4/3)*3]=下標1
但希望下標是從0開始
原本
page頁數    0         1        2
虛擬index 0,1,2     3,4,5    6,7,8
實際下標  [0,1,2]   [0,1,2]  [0,1,2]
下標往後推移,初始值由4開始
page頁數   0        1
虛擬index 4,5,6,    7
實際下標  [0,1,2]   [0]

如何取得真正下標的公式假設(虛擬index為4-4為虛擬初始值)-[第幾頁((虛擬index為4-4為虛擬初始值)/實際總筆數3)*實際總筆數3] = 實際下標0
如何取得真正下標的公式假設(虛擬index為5-4為虛擬初始值)-[第幾頁((虛擬index為5-4為虛擬初始值)/實際總筆數3)*實際總筆數3] = 實際下標1
如何取得真正下標的公式假設(虛擬index為6-4為虛擬初始值)-[第幾頁((虛擬index為6-4為虛擬初始值)/實際總筆數3)*實際總筆數3] = 實際下標2
如何取得真正下標的公式假設(虛擬index為7-4為虛擬初始值)-[第幾頁((虛擬index為7-4為虛擬初始值)/實際總筆數3)*實際總筆數3] = 實際下標0
```
{% highlight kotlin linenos %}
//虛擬數量設最大值
val virtualCount = Int.MAX_VALUE
//實際數量
val actualCount = vm.swiperData.size
//初始圖片下標，假設虛擬總筆數為8 初始值為8/2 = 4
val initialIndex = virtualCount / 2
{% endhighlight %}

{% highlight kotlin linenos %}
/**
 *擴展函式
*
 *@paramother實際總筆數
*@return
*/
private fun Int.floorMod(other:Int) : Int = when(other) {
    //index.floorMod(actualCount) 呼叫方式
    //this就是上一行註解中的index , 若總數為0 直接返回0
    0 -> this
    //(index.floorDiv(actualCount)) * actualCount
    else -> {
				//虛擬index為4-[第幾頁(虛擬index4/實際總筆數3)*實際總筆數3] = 實際下標1
        this -floorDiv(other) * other
    }
}
{% endhighlight %}

透過以下方式，呼叫上面的擴展函式
```
val actualIndex = (index - initialIndex).*floorMod*(actualCount)
```

```
如何取得真正下標的公式假設(虛擬index為4-4為虛擬初始值)-[第幾頁((虛擬index為4-4為虛擬初始值)/實際總筆數3)*實際總筆數3] = 實際下標0
```

將初始化的值變remember
{% highlight kotlin linenos %}
val pagerState = rememberPagerState(initialIndex)
{% endhighlight %}

## 輪播
import
{% highlight kotlin linenos %}
import androidx.compose.foundation.pager.HorizontalPager
import androidx.compose.foundation.pager.rememberPagerState
import coil.compose.AsyncImage
{% endhighlight %}

{% highlight kotlin linenos %}
HorizontalPager(
    pageCount = virtualCount,
    state = pagerState,
    pageSpacing = 16.dp, //圖跟圖之間有空白
    modifier = Modifier
        .padding(horizontal = 8.dp) //圖片不會貼滿左右二邊
        .clip(RoundedCornerShape(8.dp)) //圓角
){index->
	取得真正下標
	val actualIndex = (index - initialIndex).floorMod(actualCount)
	AsyncImage(
	            model = vm.swiperData[actualIndex].imageUrl,
	            contentDescription = null,
	            modifier = Modifier
	                .fillMaxWidth() //填滿
	                .aspectRatio(7 / 3f) //比例7比3
	            ,
	            contentScale = ContentScale.Crop //裁剪
	        )
}
{% endhighlight %}

## 無限輪播
{% highlight kotlin linenos %}
import java.util.Timer
import java.util.TimerTask
import androidx.compose.runtime.DisposableEffect
import androidx.compose.runtime.rememberCoroutineScope
import kotlinx.coroutines.launch
val coroutineScope = rememberCoroutineScope()
//自動輪播
//監聽什麼時候創建 什麼時候銷毀
DisposableEffect(Unit){
val timer = Timer() //創建定時器
    timer.schedule(object : TimerTask(){
        override fun run() {
            coroutineScope.launch{
								// 滾動到當前頁(currentPage)的下一頁(+1)
								pagerState.animateScrollToPage(pagerState.currentPage + 1) 
					}
        }
    },3000,3000) //每三秒循環一次
    onDispose{//銷毀定時器
        timer.cancel()
}
}
{% endhighlight %}