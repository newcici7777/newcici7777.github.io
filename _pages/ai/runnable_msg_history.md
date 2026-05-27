---
title: RunnableWithMessageHistory
date: 2026-05-26
keywords: Langchain, Gemini, RunnableWithMessageHistory
---
RunnableWithMessageHistory 可以記憶對話記錄，但只存在memory，「重新再執行」，程式不會有先前的對話記錄。<br>

使用`prompt = PromptTemplate.from_template(字串)`
{% highlight python linenos %}
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_core.runnables.history import RunnableWithMessageHistory
from langchain_core.chat_history import InMemoryChatMessageHistory
from langchain_google_genai import ChatGoogleGenerativeAI

prompt = PromptTemplate.from_template(
    "你需要根據歷史對話記錄回覆使用者問題，歷史對話記錄:{chat_history}，"
    "使用者詢問:{input}，請回答")

llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
str_parser = StrOutputParser()
store = {}

def get_history(session_id):
    if session_id not in store:
        store[session_id] = InMemoryChatMessageHistory()
    return store[session_id]

chain = prompt | llm| str_parser
conversation_chain = RunnableWithMessageHistory(
    chain,
    get_history,
    input_messages_key="input",
    history_messages_key="chat_history",
)

if __name__ == "__main__":
    session_config = {
        "configurable":{
            "session_id":"user_001",
        }
    }
    res = conversation_chain.invoke({"input":"小明有2隻貓"},session_config)
    res = conversation_chain.invoke({"input":"小花有1隻狗"},session_config)
    res = conversation_chain.invoke({"input":"總共有幾隻寵物"},session_config)
    print(res)
{% endhighlight %}
```
根據您提供的資訊，計算如下：

1. 小明有 **2 隻貓**。
2. 小花有 **1 隻狗**。

將兩者相加：2 + 1 = 3。

所以，他們總共有 **3 隻**寵物。
```

使用`prompt = ChatPromptTemplate.from_messages([])`
{% highlight python linenos %}
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import PromptTemplate, ChatPromptTemplate, MessagesPlaceholder
from langchain_core.runnables.history import RunnableWithMessageHistory
from langchain_core.chat_history import InMemoryChatMessageHistory
from langchain_google_genai import ChatGoogleGenerativeAI

prompt = ChatPromptTemplate.from_messages([
    ("system", "你需要根據歷史對話記錄回覆使用者問題"),
    MessagesPlaceholder("chat_history"),
    ("human", "使用者詢問:{input}，請回答")]
)

llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
str_parser = StrOutputParser()

store = {}

def get_history(session_id):
    if session_id not in store:
        store[session_id] = InMemoryChatMessageHistory()
    return store[session_id]

def print_prompt(full_prompt):
    print("*" * 20, full_prompt, "*" * 20)
    return full_prompt

chain = prompt | print_prompt | llm | str_parser
conversation_chain = RunnableWithMessageHistory(
    chain,
    get_history,
    input_messages_key="input",
    history_messages_key="chat_history",
)

if __name__ == "__main__":
    session_config = {
        "configurable": {
            "session_id": "user_001",
        }
    }
    res = conversation_chain.invoke({"input":"小明有2隻貓"}, session_config)
    res = conversation_chain.invoke({"input":"小花有1隻狗"}, session_config)
    res = conversation_chain.invoke({"input":"總共有幾隻寵物"}, session_config)
    print(res)

{% endhighlight %}
```
根據您提供的資訊，計算如下：

1. 小明有 **2 隻貓**。
2. 小花有 **1 隻狗**。

將兩者相加：2 + 1 = 3。

所以，他們總共有 **3 隻**寵物。
```

以下是官網範例<br>
<https://reference.langchain.com/python/langchain-core/runnables/history/RunnableWithMessageHistory><br>
