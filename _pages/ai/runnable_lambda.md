---
title: RunnableLambda
date: 2026-05-23
keywords: Langchain, Gemini, RunnableLambda
---
使用RunnableLambda，把 lambda 匿名函式的傳回值作為 prompt 輸入參數。<br>
語法:<br>
```
匿名函式變數 = RunnableLambda(lambda 參數: 傳回值)
my_func = RunnableLambda(lambda ai_msg: {"name": ai_msg.text})
```

{% highlight python linenos %}
from langchain_core.output_parsers import StrOutputParser,JsonOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_core.runnables import RunnableLambda
prompt = PromptTemplate.from_template(
    "我的鄰居姓{lastname}，剛生了{gender}，幫忙想1個名字，並轉成JSON格式給我。"
"要求key是name,value就是剛才你想的1個名字，請嚴格遵守格式要求")
second_prompt = PromptTemplate.from_template(
    "姓名{name}，請幫我解析含義。"
)
my_func = RunnableLambda(lambda ai_msg: {"name": ai_msg.text})
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
str_parser = StrOutputParser()
chain = prompt | llm | my_func | second_prompt | llm | str_parser
res = chain.invoke({"lastname": "李", "gender": "女兒"})
print(res)
{% endhighlight %}
```
這是一個非常優雅且富有文化底蘊的名字。以下是對「**李詩涵**」這個名字的詳細解析：

### 1. 單字拆解

*   **李 (Lǐ)**：
    *   **姓氏地位**：中國與台灣最常見的姓氏之一，歷史悠久，曾是唐朝的國姓，象徵著繁茂與生命力。
    *   **字義**：原意指「李樹」或「李子」。在姓名學中，李字給人一種穩重、紮根大地的感覺。

*   **詩 (Shī)**：
    *   **字義**：指詩歌、詩句。
    *   **寓意**：代表**才華、文學修養、浪漫與靈性**。這個字賦予孩子一種文靜、優雅的氣質，象徵生活如詩般美好，擁有豐富的內心世界與藝術感。

*   **涵 (Hán)**：
    *   **字義**：原意為水入船中，後引申為包容、蘊含、滋潤（如：涵養、包涵）。
    *   **寓意**：代表**深度、修養與包容力**。這個字暗示了一個人有內涵、有禮貌，性格溫潤如水，處事大方且具備良好的自我約束能力。
```

RunnableLambda 父類別是 Runnable，而Runnable 的 `__or__()`方法參數接受Callable類型。<br>
而lambda語法，父類別是Callable，所以也可以直接把Lambda語法放入chain。<br>
{% highlight python linenos %}
    def __or__(
        self,
        other: Runnable[Any, Other]
        | Callable[[Iterator[Any]], Iterator[Other]]
        | Callable[[AsyncIterator[Any]], AsyncIterator[Other]]
        | Callable[[Any], Other]
        | Mapping[str, Runnable[Any, Other] | Callable[[Any], Other] | Any],
    ) -> RunnableSerializable[Input, Other]:
        """Runnable "or" operator.

        Compose this `Runnable` with another object to create a
        `RunnableSequence`.

        Args:
            other: Another `Runnable` or a `Runnable`-like object.

        Returns:
            A new `Runnable`.
        """
        return RunnableSequence(self, coerce_to_runnable(other))

{% endhighlight %}

lambda 放入chain。<br>
{% highlight python linenos %}
from langchain_core.output_parsers import StrOutputParser,JsonOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_core.runnables import RunnableLambda
prompt = PromptTemplate.from_template(
    "我的鄰居姓{lastname}，剛生了{gender}，幫忙想1個名字，並轉成JSON格式給我。"
"要求key是name,value就是剛才你想的1個名字，請嚴格遵守格式要求")
second_prompt = PromptTemplate.from_template(
    "姓名{name}，請幫我解析含義。"
)
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
str_parser = StrOutputParser()
chain = prompt | llm | (lambda ai_msg: {"name": ai_msg.text}) | second_prompt | llm | str_parser
res = chain.invoke({"lastname": "李", "gender": "女兒"})
print(res)
{% endhighlight %}