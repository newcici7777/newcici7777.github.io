---
title: BST刪除
date: 2025-07-31
keywords: java,Binary Search Tree
---
## root節點刪除
1. 先判斷要刪除的節點是不是root節點。
2. 不是root，那就是子節點。

## 子節點刪除
單向鏈結串列刪除時，要先找到刪除節點的前一個節點，因為單向鏈結串列無法往回走。<br>
樹的刪除也是一樣，要先找到「要刪除節點」的「父節點」。<br>
「要刪除節點」的「父節點」，命名為parent。<br>
「要刪除節點」，命名為target。<br>

### 葉子節點刪除
![img]({{site.imgurl}}/java_datastruct/bst_del_leaf.png)<br>
要刪除的節點是葉子，檢查要刪除的節點是左子節點，還是右子節點。<br>
直接設為null。<br>

要刪除的節點是左子節點，parent為「要刪除節點」的「父節點」。<br>
{% highlight java linenos %}
parent.left = null;
{% endhighlight %}

要刪除的節點是右子節點，parent為「要刪除節點」的「父節點」。<br>
{% highlight java linenos %}
parent.right = null;
{% endhighlight %}

### 要刪除的節點，是左子節點，下面有一個子節點
如下圖，要刪除的節點，是左子節點，下面有一個子節點。
![img]({{site.imgurl}}/java_datastruct/bst_del_1child.png)<br>

要刪除的節點，是左子節點，下面有一個左子節點。

刪除後的圖如下:<br>
![img]({{site.imgurl}}/java_datastruct/bst_del_l_1chl.png)<br>

{% highlight java linenos %}
parent.left = target.left;
{% endhighlight %}

要刪除的節點，是左子節點，下面有一個右子節點。

刪除後的圖如下:<br>
![img]({{site.imgurl}}/java_datastruct/bst_del_l_1chr.png)<br>

{% highlight java linenos %}
parent.left = target.right;
{% endhighlight %}

名詞解釋:<br>
parent為「要刪除節點」的「父節點」。<br>
parent.left為「要刪除節點」的「父節點」下的左子節點。<br>
「要刪除節點」，命名為target。<br>
target.left為要刪除節點的左子節點。<br>
target.right為要刪除節點的右子節點。<br>

### 要刪除的節點，是右子節點，下面有一個子節點
要刪除的節點，是右子節點，下面有一個左子節點。

刪除後的圖如下:<br>
![img]({{site.imgurl}}/java_datastruct/bst_del_r_1chl.png)<br>

{% highlight java linenos %}
parent.right = target.left;
{% endhighlight %}

要刪除的節點，是右子節點，下面有一個右子節點。

刪除後的圖如下:<br>
![img]({{site.imgurl}}/java_datastruct/bst_del_r_1chr.png)<br>

{% highlight java linenos %}
parent.right = target.right;
{% endhighlight %}

### 要刪除的節點，有2個子節點
尋找右子樹中最小值，使用temp變數，暫存最小值。
![img]({{site.imgurl}}/java_datastruct/bst_del_2ch_1.png)<br>

把最小節點8刪掉。
![img]({{site.imgurl}}/java_datastruct/bst_del_2ch_2.png)<br>

把要刪除節點的值，修改成temp變數最小值。
![img]({{site.imgurl}}/java_datastruct/bst_del_2ch_3.png)<br>