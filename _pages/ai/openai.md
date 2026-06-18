---
title: openai
date: 2026-05-14
keywords: openai
---
## PyPI
PyPI 全名為Python Package Index 第三方套件倉庫。<br>

pip 為Python 套件安裝工具，支援下載、安裝、移除。<br>

1 進入python 官網
<https://www.python.org/>

2.點擊最上方Pypi 
![img]({{site.imgurl}}/ai/pipy1.png)<br>

搜尋openai
![img]({{site.imgurl}}/ai/pipy2.png)<br>

搜尋出來的網址:<https://pypi.org/project/openai/>

拉至網頁下方，會有相關openai的使用方法。

![img]({{site.imgurl}}/ai/pipy3.png)<br>

## 終端機安裝open ai
Pycharm打開終端機，安裝open ai<br>
進入終端機<br>
![img]({{site.imgurl}}/python/venv.png)<br>

安裝 openai，有二種方式:<br>
不設定版本，下載最新版本。
```
pip3 install openai
```

設定版本
```
pip3 install openai=設定版本
pip3 install openai=2.13.0
```

## openai gemini 使用方法
參考網址<https://ai.google.dev/gemini-api/docs/openai?hl=zh-tw><br>

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

## 讀取環境變數取得OPENAI_API_KEY

MAC設置OPENAI_API_KEY變數
```
vi ~/.zshrc
```

增加OPENAI_API_KEY。
```
export OPENAI_API_KEY="xxx"
```

更新設定
```
source ~/.zshrc
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
    stream=True # 串流輸出, 注意是大寫的True
)

# 3. 串流輸出結果
for chunk in completion:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
{% endhighlight %}

## 使用 os.environ 取得自己想要的KEY

{% highlight python linenos %}
import os

from openai import OpenAI

# 1. 初始化 Client
client = OpenAI(
    api_key= os.environ["GOOGLE_API_KEY"],
    # Gemini 提供的 OpenAI 兼容 Base URL
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)

# 2. 建立對話請求
response = client.chat.completions.create(
    # 使用 Gemini 的模型名稱
    model="gemini-3-flash-preview",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "你好，這是一個使用 OpenAI SDK 的 Gemini API 測試。"}
    ],
    stream= False # 非串流，注意！是大寫的False
)

# 3. 串流輸出結果
print(response.choices[0].message.content)
{% endhighlight %}
```
你好！收到了，這是一個非常成功的測試。

看來你已經成功透過 OpenAI SDK 建立了與 Gemini API 的連線。這種兼容性讓開發者可以非常方便地在不同的模型之間切換，而不需要大幅修改程式碼。

如果你需要進一步測試以下功能，請隨時告訴我：
1. **流式輸出 (Streaming)**：測試文字是否能即時傳回。
2. **多輪對話 (Chat History)**：測試模型是否能記得之前的脈絡。
3. **系統提示詞 (System Message)**：測試模型是否能遵循特定的角色設定。

請問還有什麼我可以協助你的嗎？
````


DASHSCOPE_API_KEY(是千問)。
