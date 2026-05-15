---
title: openai
date: 2026-05-14
keywords: openai
---
MAC安裝open ai
```
pip3 install openai
```

參考網址<https://ai.google.dev/gemini-api/docs/openai?hl=zh-tw>

pycharm要安裝openai
{% highlight python linenos %}
from openai import OpenAI

# 1. 初始化 Client
client = OpenAI(
    # 填入你從 Google AI Studio 申請的 API Key
    api_key="xxx",
    # Gemini 提供的 OpenAI 兼容 Base URL
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)

# 2. 建立對話請求
completion = client.chat.completions.create(
    # 使用 Gemini 的模型名稱 
    model="gemini-3-flash-preview",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "你好，這是一個使用 OpenAI SDK 的 Gemini API 測試。"}
    ],
    stream=True
)

# 3. 串流輸出結果
for chunk in completion:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
{% endhighlight %}
```
你好！收到你的測試訊息了。

這代表你使用 OpenAI SDK 串接 Gemini API 的設定已經成功運作。我是 Gemini，很高興能透過這個相容介面與你交流。

如果你需要進一步測試不同的功能（例如：生成文本、翻譯、程式碼撰寫或是角色扮演），請隨時告訴我！有什麼我可以幫你的嗎？
```

MAC設置OPENAI_API_KEY變數
```
vi ~/.zshrc
```

增加OPENAI_API_KEY。
```
export OPENAI_API_KEY="xxx"
```

把Pycharm重啟，去掉api_key="xxx"，會自動讀取電腦的OPENAI_API_KEY變數。
{% highlight python linenos %}
from openai import OpenAI

# 1. 初始化 Client
client = OpenAI(
    # Gemini 提供的 OpenAI 兼容 Base URL
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)

# 2. 建立對話請求
completion = client.chat.completions.create(
    # 使用 Gemini 的模型名稱 
    model="gemini-3-flash-preview",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "你好，這是一個使用 OpenAI SDK 的 Gemini API 測試。"}
    ],
    stream=True
)

# 3. 串流輸出結果
for chunk in completion:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
{% endhighlight %}

DASHSCOPE_API_KEY(是千問)。
