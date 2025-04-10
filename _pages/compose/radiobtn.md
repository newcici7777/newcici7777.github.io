---
title: RadioButton
date: 2023-05-03
keywords: Android, Jetpack compose, RadioButton
---
{% highlight kotlin linenos %}
//單個按鈕
var selected by remember{
	mutableStateOf(false) //Ò默認為false
}
RadioButton(selected = selected, onClick ={
  //預設false，點擊變true
  selected = !selected
})
{% endhighlight %}

{% highlight kotlin linenos %}
var checkedList by remember {
  mutableStateOf(listOf(false, false)) //2個RadioButton，預設false
}
Column() {
  checkedList.forEachIndexed { index, item ->
    RadioButton(selected = item, onClick = {
      //透過index判斷當前點擊的是那一個
      checkedList = checkedList.mapIndexed{
        j,_ -> index == j
      }

    })
  }
}
{% endhighlight %}
![img]({{site.imgurl}}/compose/radiobtn.png)   
