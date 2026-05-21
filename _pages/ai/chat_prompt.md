---
title: ChatPromptTemplate
date: 2026-05-20
keywords: Langchain, Gemini, ChatPromptTemplate
---
## llm.invoke
ChatPromptTemplate 有 記憶功能 MessagesPlaceholder。<br>

{% highlight python linenos %}
from langchain_core.prompts import ChatPromptTemplate,MessagesPlaceholder
from langchain_google_genai import ChatGoogleGenerativeAI

chat_prompt_template = ChatPromptTemplate.from_messages(
    [
        ("system","你是一個詩人"),
        MessagesPlaceholder("history"),
        ("human","請再來一首唐詩")
    ]
)
history_data = [
    ("human", "你寫一首詩"),
    ("ai", "鋤禾日當午，汗滴禾下土。誰知盤中飧，粒粒皆辛苦。"),
]
prompt_text = chat_prompt_template.invoke({"history": history_data}).to_string()
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
res = llm.invoke(input=prompt_text)
print(res.text)
{% endhighlight %}
```
既然你喜愛唐詩，那我再為你吟誦一首李白的經典之作，這首詩描繪了旅人的思鄉之情：

**《靜夜思》 李白**

床前明月光，
疑是地上霜。
舉頭望明月，
低頭思故鄉。
```

## 使用chain

語法:
```
chat_prompt_template = ChatPromptTemplate.from_messages(
    [
        ...
        MessagesPlaceholder("history"),
        ...
    ]
)
history_data = [...]
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
chain = chat_prompt_template | llm
res = chain.invoke({"history": history_data})
```

{% highlight python linenos %}
from langchain_core.prompts import ChatPromptTemplate,MessagesPlaceholder
from langchain_google_genai import ChatGoogleGenerativeAI

chat_prompt_template = ChatPromptTemplate.from_messages(
    [
        ("system","你是一個詩人"),
        MessagesPlaceholder("history"),
        ("human","請再來一首唐詩")
    ]
)
history_data = [
    ("human", "你寫一首詩"),
    ("ai", "鋤禾日當午，汗滴禾下土。誰知盤中飧，粒粒皆辛苦。"),
]

llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
chain = chat_prompt_template | llm
res = chain.invoke({"history": history_data})
print(res.text)
{% endhighlight %}
```
既然客官有此雅興，那便再為您吟誦一首李太白的千古絕唱：

**《靜夜思》**
**李白**

**床前明月光，**
**疑是地上霜。**
**舉頭望明月，**
**低頭思故鄉。**

這窗外的月色，是否也勾起了您的思緒？若還想聽，我這兒還有許多詩篇。
```