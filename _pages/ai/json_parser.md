---
title: JsonOutputParser
date: 2026-05-21
keywords: Langchain, JsonOutputParser
---
{% highlight python linenos %}
from langchain_core.output_parsers import StrOutputParser,JsonOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

prompt = PromptTemplate.from_template(
    "我的鄰居姓{lastname}，剛生了{gender}，幫忙想1個名字，並轉成JSON格式給我。"
"要求key是name,value就是剛才你想的1個名字，請嚴格遵守格式要求")
second_prompt = PromptTemplate.from_template(
    "姓名{name}，請幫我解析含義。"
)
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
str_parser = StrOutputParser()
json_parser = JsonOutputParser()
chain = prompt | llm | json_parser | second_prompt | llm | str_parser
res = chain.invoke({"lastname": "李", "gender": "女兒"})
print(res)
{% endhighlight %}
```
「**李沐恩**」是一個非常有氣韻、溫文儒雅且寓意深遠的名字。以下為您從字義、文化內涵、音韻以及五行等維度進行詳細解析：

### 1. 單字解析

*   **李 (Lǐ)：**
    *   **姓氏：** 中國大姓，象徵著繁衍興旺。
    *   **原義：** 指李樹、李子。在文化中，李花象徵純潔，李果象徵紮實與收穫。
*   **沐 (Mù)：**
    *   **原義：** 原指洗頭髮，引申為沐浴、潤澤、受薰陶。
    *   **意境：** 給人一種清新、乾淨、被滋潤的感覺。如「如沐春風」，形容受到良好的教導或處於優美的環境中。
*   **恩 (Ēn)：**
    *   **原義：** 恩惠、恩德、慈愛。
    *   **意境：** 代表著一顆感恩的心，也象徵此人一生多得貴人相助，承蒙上天的眷顧與恩寵。
```