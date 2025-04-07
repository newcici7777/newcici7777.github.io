---
title: Compose Text
date: 2025-04-06
keywords: Android, Jetpack compose, Compose Text
---
先建立package叫Components. 
![img]({{site.imgurl}}/compose/compose_text1.png)

再建立Kotlin File名字TextSample  
![img]({{site.imgurl}}/compose/compose_text2.png)

建立Text
{% highlight kotlin linenos %}
package com.example.myapplication.components
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.FontFamily
import androidx.compose.ui.text.font.FontStyle
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.text.style.TextDecoration
import androidx.compose.ui.text.style.TextOverflow
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.ExperimentalUnitApi
import androidx.compose.ui.unit.TextUnit
import androidx.compose.ui.unit.TextUnitType
import androidx.compose.ui.unit.sp
import com.example.myapplication.R
@OptIn(ExperimentalUnitApi::class)
@Composable
fun TextSample() {
    Text(
        text = stringResource(id = R.string.testData),
        color = Color(255, 255, 255),
        //fontSize = TextUnit(16f, TextUnitType.Sp)
        fontSize = 16.sp,
        fontStyle = FontStyle.Italic,//斜體
        fontWeight = FontWeight.Bold,//粗體
        fontFamily = FontFamily.SansSerif,//字型
        letterSpacing = 10.sp,//字之間的距離
        //底線跟中線
        textDecoration = TextDecoration.combine(
            listOf(
                TextDecoration.LineThrough,
                TextDecoration.Underline
            )
        ),
        //文字居右
        textAlign = TextAlign.Center,
        //行高
        lineHeight = 30.sp,
        maxLines = 1,//最大行數
        //超出就...
        overflow = TextOverflow.Ellipsis,
        style = TextStyle()
    )
}
@Preview(widthDp = 100)
@Composable
fun TextSamplePreview() {
    TextSample()
}
{% endhighlight %}

建立click以及sytle跟push annotation
{% highlight kotlin linenos %}
package com.example.myapplication.components
import android.util.Log
import androidx.compose.foundation.text.ClickableText
import androidx.compose.runtime.Composable
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.SpanStyle
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.buildAnnotatedString
import androidx.compose.ui.text.style.TextDecoration
import androidx.compose.ui.text.withStyle
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.ExperimentalUnitApi
import androidx.compose.ui.unit.sp
@OptIn(ExperimentalUnitApi::class)
@Composable
fun TextSample2() {
    val annotationStr = buildAnnotatedString {
        append("text text")
        pushStringAnnotation("tag1", "http://xxxx.xxxx/xx")
        withStyle(
            style = SpanStyle(
                color = Color.Red,
                textDecoration = TextDecoration.Underline,
                fontSize = 16.sp
            )
        ) {
            append("tag1")
        }
        pop()
        append("ggg")
        pushStringAnnotation("tag2", "http://123.xxxx/123")
        withStyle(
            style = SpanStyle(
                color = Color.Red,
                textDecoration = TextDecoration.Underline,
                fontSize = 16.sp
            )
        ) {
            append("tag2")
        }
        pop()
    }
    ClickableText(text = annotationStr, onClick = { offset ->
        annotationStr.getStringAnnotations("tag1", start = offset, end = offset).firstOrNull()
            ?.let { annotation ->
                Log.d("///aaaa", "test${annotation.item}")
            }
        annotationStr.getStringAnnotations("tag2", start = offset, end = offset).firstOrNull()
            ?.let { annotation ->
                Log.d("///aaaa", "test${annotation.item}")
            }
    })
}
@Preview
@Composable
fun TextSample2Preview() {
    TextSample2()
}
{% endhighlight %}