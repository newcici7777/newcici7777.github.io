---
title: Chat model
date: 2026-05-20
keywords: Langchain, Gemini, Chat model, HumanMessage, SystemMessage
---
- SystemMessage 設定AI的背景。<br>
- HumanMessage 向AI提出問題。<br>
- AIMessage 訓練AI回應的答案。<br>
{% highlight python linenos %}
from langchain_core.messages import HumanMessage, SystemMessage, AIMessage
from langchain_google_genai import ChatGoogleGenerativeAI

# 初始化 Gemini LLM
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
messages = [
    SystemMessage(content="你是一個詩人"),
    HumanMessage(content="給我寫一首唐詩"),
    AIMessage(content="鋤禾日當午，汗滴禾下土。誰知盤中飧，粒粒皆辛苦。"),
    HumanMessage(content="根據上一首詩的格式，再來一首。"),
]
for chunk in llm.stream(input=messages):
    print(chunk.text)
{% endhighlight %}
```
既然上一首是描
繪農家辛勞的五言絕句，我便為你再作一首風格相近、感
懷時事的五言絕句：

**《蠶婦》**

**昨日入城市，**
**
歸來淚滿巾。**
**遍身羅綺者，**
**不是養蠶人。**
```

以下方式跟上面是相同。
{% highlight python linenos %}
from langchain_core.messages import HumanMessage, SystemMessage, AIMessage
from langchain_google_genai import ChatGoogleGenerativeAI

# 初始化 Gemini LLM
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)

messages = [
    ("system", "你是一個詩人"),
    ("human", "給我寫一首唐詩"),
    ("ai", "鋤禾日當午，汗滴禾下土。誰知盤中飧，粒粒皆辛苦。"),
    ("human", "根據上一首詩的格式，再來一首。")
]

for chunk in llm.stream(input=messages):
    print(chunk.text)
{% endhighlight %}
```
既然承接上一首《憫農》的格式（五言絕句
），我便為你再賦一首同為李紳所作、意境相連的姊妹篇
：

**《憫農》（其一）**

**春種一粒粟，**
**秋收萬
顆子。**
**四海無閒田，**
**農夫猶餓死。**

---


**【詩人自述】**
這首詩與上一首格式相同，皆為五言絕
句。它描繪了諷刺而悲哀的對比：春天種下一粒種子，秋天收
穫萬顆糧食，普天之下已沒有荒廢的田地，但辛勤耕作的農夫
卻依然面臨餓死的命運。語氣平實，卻字字千鈞。
```