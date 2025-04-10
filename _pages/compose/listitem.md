---
title: ListItem
date: 2023-05-03
keywords: Android, Jetpack compose, ListItem
---
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