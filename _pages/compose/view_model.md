---
title: View Model
date: 2023-06-01
keywords: Android, Jetpack compose, View Model
---
參考以下網址
[https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=zh-cn](https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=zh-cn)  

將lifecycle的包打入  
![img]({{site.imgurl}}/compose/view_model1.png)  
```
var lifecycle_version = "2.5.1"
// Lifecycles only (without ViewModel or LiveData)
implementation("androidx.lifecycle:lifecycle-runtime-ktx:$lifecycle_version")
// ViewModel utilities for Compose
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:$lifecycle_version")
```

建立`viewmodel`的package，在java目錄下按滑鼠右鍵"New"→"package"<br>
![img]({{site.imgurl}}/compose/viewmodel1.png)<br>

在viewmodel的目錄下建立xxViewModel.kt。<br>
![img]({{site.imgurl}}/compose/viewmodel2.png)<br>

![img]({{site.imgurl}}/compose/viewmodel3.png)<br>

## 使用mutableState建立ViewModel
{% highlight kotlin linenos %}
package viewmodel

import androidx.compose.runtime.mutableStateListOf
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.launch

class MyViewModel : ViewModel(){
  private val _items = mutableStateListOf<String>()
  val items: List<String> = _items
  init {
    loadItems()
  }
  private fun loadItems() {
    viewModelScope.launch {
      _items.addAll(List(500) { "Item $it" })
    }
  }
}
{% endhighlight %}

在Compose建立viewModel跟Activity建立viewModel是不同，Compose使用`=`指派的方式，Android使用by的方式。<br>
Compose ViewModel語法:<br>
```
@Composable
fun lazyColumnExample7() {
  val viewModel: MyViewModel = viewModel()
}
```

Android ViewModel語法:<br>
```
class MainActivity08 : AppCompatActivity() {
  private val viewModel by viewModels<NumberViewModel>()
}
```

{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample7() {
  val viewModel : MyViewModel = viewModel()
  LazyColumn{
    items(viewModel.items) { item ->
      Text("$item")
    }
  }
}
{% endhighlight %}


## 使用MutableStateFlow建立ViewModel
ViewModel
{% highlight kotlin linenos %}
package viewmodel

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.launch

class MyViewModel2 : ViewModel(){
  private val _items = MutableStateFlow<List<String>>(emptyList())
  val items: StateFlow<List<String>> = _items
  init {
    loadItems()
  }
  private fun loadItems() {
    viewModelScope.launch {
      _items.value = List(500) { "Item $it" }
    }
  }
}
{% endhighlight %}

Compose
{% highlight kotlin linenos %}
@Composable
fun lazyColumnExample8() {
  val viewModel: MyViewModel2 = viewModel()
  val items by viewModel.items.collectAsState()
  LazyColumn {
    items(items) { item ->
      Text("$item")
    }
  }
}
{% endhighlight %}
----------------------------------------------------
以下是舊文章

dataClass
![img]({{site.imgurl}}/compose/view_model2.png)  
![img]({{site.imgurl}}/compose/view_model3.png)  
![img]({{site.imgurl}}/compose/view_model4.png)  
建view
![img]({{site.imgurl}}/compose/view_model5.png)  
![img]({{site.imgurl}}/compose/view_model6.png)  
![img]({{site.imgurl}}/compose/view_model7.png)  