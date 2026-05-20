---
title: PromptTemplate
date: 2026-05-20
keywords: Langchain, Gemini, PromptTemplate
---
語法
```
prompt_template.format()
```

{% highlight python linenos %}
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

prompt_template = PromptTemplate.from_template(
    "我的鄰居姓{lastname}，剛生了{gender}，幫忙想名字，簡短回答。"
)
prompt_text = prompt_template.format(lastname="李", gender="女兒")
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
res = llm.invoke(input=prompt_text)
print(res.text)
{% endhighlight %}
```
恭喜李先生/太太！以下為您精選幾個適合李姓女寶寶的名字：

**【氣質優雅】**
1. **李若汐**（溫柔如水）
2. **李芷嫣**（清新脫俗）
3. **李婉清**（古典婉約）

**【清新自然】**
4. **李沐希**（充滿希望）
5. **李雨霏**（詩情畫意）
6. **李沁怡**（沁人心脾）

**【簡約大方】**
7. **李悅**（快樂喜悅）
8. **李予**（給予、大氣）
9. **李汐**（潮汐、靈動）

**【聰慧靈動】**
10. **李思齊**（見賢思齊）
11. **李知恩**（懂得感恩）
12. **李藝涵**（有才華涵養）

建議選定後，再配合寶寶的出生時辰（八字）微調。
```

使用chain的方式
{% highlight python linenos %}
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

prompt_template = PromptTemplate.from_template(
    "我的鄰居姓{lastname}，剛生了{gender}，幫忙想名字，簡短回答。"
)
prompt_text = prompt_template.format(lastname="李", gender="女兒")
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
chain = prompt_template | llm
res = llm.invoke(input=prompt_text)
print(res.text)
{% endhighlight %}
```
恭喜李先生/太太！以下為您精選幾個適合李姓女寶寶的名字：

**【氣質優雅】**
1. **李若汐**（溫柔如水）
2. **李芷嫣**（清新脫俗）
3. **李婉清**（古典婉約）

**【清新自然】**
4. **李沐希**（充滿希望）
5. **李雨霏**（詩情畫意）
6. **李沁怡**（沁人心脾）

**【簡約大方】**
7. **李悅**（快樂喜悅）
8. **李予**（給予、大氣）
9. **李汐**（潮汐、靈動）

**【聰慧靈動】**
10. **李思齊**（見賢思齊）
11. **李知恩**（懂得感恩）
12. **李藝涵**（有才華涵養）

建議選定後，再配合寶寶的出生時辰（八字）微調。
```