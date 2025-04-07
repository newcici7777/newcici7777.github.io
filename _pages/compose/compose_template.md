---
title: Compose Template
date: 2023-04-21
keywords: Android, Jetpack compose
---
{% highlight kotlin linenos %}
{% endhighlight %}
![img]({{site.imgurl}}/compose/compose_template1.png)
![img]({{site.imgurl}}/compose/compose_template2.png)
{% highlight kotlin linenos %}
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME}
#end
#parse("File Header.java")
import androidx.compose.runtime.Composable
import androidx.compose.ui.tooling.preview.Preview
#if (${Function_Name} == "" )
@Composable
fun ${NAME}() {
}
@Preview
@Composable
fun ${NAME}Preview() {
    ${NAME}()
}
#end
#if (${Function_Name} != "" )
@Composable
fun ${Function_Name}() {
}
@Preview
@Composable
fun ${Function_Name}Preview() {
    ${Function_Name}()
}
#end
{% endhighlight %}
![img]({{site.imgurl}}/compose/compose_template3.png)
![img]({{site.imgurl}}/compose/compose_template4.png)
![img]({{site.imgurl}}/compose/compose_template5.png)
![img]({{site.imgurl}}/compose/compose_template6.png)
{% highlight kotlin linenos %}
@androidx.compose.runtime.Composable
fun $NAME$() {
$END$
}
@androidx.compose.ui.tooling.preview.Preview
@androidx.compose.runtime.Composable
fun $NAME$Preview() {
$END$
}
{% endhighlight %}
