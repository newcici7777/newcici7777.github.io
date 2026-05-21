---
title: StrOutputParser
date: 2026-05-21
keywords: Langchain, StrOutputParser
---
## 輸入輸出
llm 大模型「輸入」類型只能接受 str字串、list、dict、tuple、BaseMessage。<br>
llm 大模型「輸出」類型是AIMessage。<br>
要把輸出類型是AIMessage，轉成llm能接受的輸入類型字串。<br>
再次把字串輸入llm。<br>

## invoke 傳回值是AIMessage
{% highlight python linenos %}
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

prompt = PromptTemplate.from_template(
    "我的鄰居姓{lastname}，剛生了{gender}，幫忙想3個名字，只要告知名字，無需其它內容。")
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
chain = prompt | llm
res = chain.invoke({"lastname": "李", "gender": "女兒"})
print(type(res))
{% endhighlight %}
```
<class 'langchain_core.messages.ai.AIMessage'>
```

## StrOutputParser
上一個例子中，res 是 AIMessage 類別，沒辦法作為llm的參數，所以要把AIMessage的類別，透過StrOutputParser變成字串，再作為字串參數傳入llm。<br>
{% highlight python linenos %}
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

prompt = PromptTemplate.from_template(
    "我的鄰居姓{lastname}，剛生了{gender}，幫忙想3個名字，只要告知名字，無需其它內容。")
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
str_parser = StrOutputParser()
chain = prompt | llm | str_parser | llm
res = chain.invoke({"lastname": "李", "gender": "女兒"})
print(res.text)
{% endhighlight %}
```
这三个名字都非常优美好听，属于典型的**现代、温婉、且富有书卷气**的女孩名字。它们都选用了比较清新的汉字，给人一种大方、灵动的感觉。

以下是对这三个名字的详细解析和对比建议：

### 1. 李若曦 (Lǐ Ruòxī)
*   **字义解析：**
    *   **若：** 意为“好像”，常用于形容某种美好的状态。
    *   **曦：** 指早晨的阳光，象征希望、光明和朝气。
*   **名字寓意：** 像清晨的阳光一样灿烂、温暖。这个名字给人一种**明亮、高雅、充满生命力**的感觉。
*   **风格：** 偏向“言情小说女主”风，非常唯美，但也因为“曦”字在近年起名中较为热门，重名率相对稍高。
*   **建议：** 如果你希望孩子性格开朗、阳光，这个名字非常合适。

### 2. 李芷晴 (Lǐ Zhǐqíng)
*   **字义解析：**
    *   **芷：** 指白芷，一种香草，在古代文学中常喻指高尚的品德。
    *   **晴：** 天空无云，阳光明媚。
*   **名字寓意：** 拥有如香草般芬芳高洁的品格，且性格像晴天一样明朗。这个名字结合了**草本的清香和天气的明快**。
*   **风格：** 比较**清新、自然、知性**。相比“若曦”，它多了一份稳重和草木的灵气。
*   **建议：** 这是一个非常耐听的名字，既有古典韵味又不失现代感。

### 3. 李芯宜 (Lǐ Xīnyí)
*   **字义解析：**
    *   **芯：** 原指草木的中心，也常用于形容精髓、核心。在名字中给人一种玲珑、聪慧的感觉。
    *   **宜：** 意为合适、应当、宜人。
*   **名字寓意：** 寓意孩子是一个贴心、得体、让人感到舒适的人。这个名字强调的是**和谐、平衡和内在的美好**。
*   **风格：** 更加**温柔、内敛、乖巧**。读音上非常柔和，给人一种邻家女孩的亲切感。
*   **建议：** 如果希望孩子性格温婉、处事大方得体，这个名字是极好的选择。
```