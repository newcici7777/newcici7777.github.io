---
title: ListItem
date: 2023-05-03
keywords: Android, Jetpack compose, ListItem
---

![img]({{site.imgurl}}/compose/listitem1.png)  <br>

{% highlight kotlin linenos %}
@Composable
fun ListItemExample1() {
  Column {
    ListItem(
      // 標題必填
      headlineContent = {
        Text("Title")
      },
      // 內容(非必填)
      supportingContent = {
        Text("Content")
      },
      // 前面圖案(非必填)
      leadingContent = {
        Icon(imageVector = Icons.Default.Favorite, contentDescription = null)
      },
      // 後面圖案或文字或其它元件(非必填)
      trailingContent = {
        Icon(imageVector = Icons.Default.ArrowUpward, contentDescription = null)
      },
      // 標題上方小文字(非必填)
      overlineContent = {
        Text("Category")
      }
    )
  }
}
{% endhighlight %}

---------------------------------------
以下舊文章
{% highlight kotlin linenos %}
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ListItemSample() {
  var list by remember{mutableStateOf(listOf(
        false, false, false, false, false)
    )
}
Column(){
list.forEachIndexed(){rawIndex, listItem->
ListItem(
  leadingContent ={Icon(Icons.Default.AccountBox,
  contentDescription = "Localized description")
  },
  headlineText ={
    Text(text = "ccc $rawIndex")
  },
  supportingText ={
    Text(text = "aaa")
  },
  trailingContent ={
    Checkbox(checked = listItem, onCheckedChange ={
          list = list.mapIndexed{newIndex, listItem->
                if (rawIndex == newIndex) {
           !listItem
        } else {
           listItem
        }
          }
          Log.i("====", "${list}")
    })
  },
  overlineText ={
    Text(text = "ggg")
  })
  } // end of forEachIndexed
  } // end of ListItem
{% endhighlight %}
![img]({{site.imgurl}}/compose/listitem.png)   