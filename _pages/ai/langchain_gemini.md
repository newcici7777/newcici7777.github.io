---
title: Langchain Gemini
date: 2026-05-15
keywords: Langchain, Gemini
---
## 安裝langchain
以下是在MAC安裝，Windows請用pip，而不是pip3。<br>
```
pip3 install langchain
pip3 install langchain-core
```

如果llms沒有在以下網址
<https://langchain.cadn.net.cn/python/docs/integrations/providers/index.html>
安裝langchain-community
```
pip3 install langchain-community
```

## 安裝 langchain-google-gena
MAC安裝
```
pip3 install langchain-google-genai langchain
```

MAC設置OPENAI_API_KEY變數
```
vi ~/.zshrc
```

增加GOOGLE_API_KEY
```
export GOOGLE_API_KEY="xxx"
```

說明文件:<br>
<https://reference.langchain.com/python/langchain-google-genai/chat_models/ChatGoogleGenerativeAI>
<br>
<https://reference.langchain.com/python/langchain-google-genai>
<br>

## invoke
執行後，回覆全部出來。<br>
{% highlight python linenos %}
from langchain_google_genai import ChatGoogleGenerativeAI

# 初始化 Gemini LLM
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
res = llm.invoke(input="你是誰")
print(res)
{% endhighlight %}

## stream
執行後，回覆陸續出來。<br>
{% highlight python linenos %}
from langchain_google_genai import ChatGoogleGenerativeAI

# 初始化 Gemini LLM
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
res = llm.stream(input="你是誰")
for chunk in res:
    print(chunk, end="", flush=True)
{% endhighlight %}