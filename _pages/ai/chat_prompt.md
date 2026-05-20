---
title: ChatPromptTemplate
date: 2026-05-20
keywords: Langchain, Gemini, ChatPromptTemplate
---
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