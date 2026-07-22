---
title: Chat with AI
date: 2026-07-22
keywords: openai,streamlit
---
## session_state
記錄使用者與AI的對話

document路徑

API reference/Caching and state/st.session_state

![img]({{site.imgurl}}/streamlit/session_state.png)<br>

{% highlight python linenos %}
# 初始化messages
if "messages" not in st.session_state:
    st.session_state.messages = []

# 將之前的對話匯入chat_message
for message in st.session_state.messages:
    st.chat_message(message["role"]).write(message["content"])
{% endhighlight %}

![img]({{site.imgurl}}/streamlit/chat_ai1.png)<br>

{% highlight python linenos %}
import streamlit as st
from openai import OpenAI

st.set_page_config(
    page_title="Streamlit Example",
    page_icon="😁",
    layout="wide",
    initial_sidebar_state="expanded",
    menu_items={
        "About": "This is a example page.",
        "Get Help": "https://streamlit.io/",
    }
)

system_prompt = "You are a helpful assistant."

client = OpenAI(
    # Gemini 提供的 OpenAI 兼容 Base URL
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)

if "messages" not in st.session_state:
    st.session_state.messages = []

for message in st.session_state.messages:
    st.chat_message(message["role"]).write(message["content"])

prompt = st.chat_input("請問你要問什麼問題")
if prompt:
    st.chat_message("user").write(prompt)
    # 儲存user的prompt
    st.session_state.messages.append({"role": "user", "content": prompt})
    print([
        {"role": "system", "content": system_prompt},
        *st.session_state.messages
    ])
    response = client.chat.completions.create(
        # 使用 Gemini 的模型名稱
        model="gemini-3-flash-preview",
        messages=[
            {"role": "system", "content": system_prompt},
            *st.session_state.messages
        ],
        stream=False  # 串流輸出, 注意是大寫的True
    )
    st.chat_message("assistant").write(response.choices[0].message.content)
    # 儲存ai回覆
    st.session_state.messages.append({"role": "assistant", "content": response.choices[0].message.content})
{% endhighlight %}

## stream為True
當輸出方式為邊回答邊輸出，需要使用到empty()空容器

<https://docs.streamlit.io/develop/api-reference/layout/st.empty>

位置在 API reference/Layouts and containers/st.empty

使用方法如下:
{% highlight python linenos %}
st.empty()
response_message.chat_message("assistant").write(full_response)
{% endhighlight %}

完整程式碼
{% highlight python linenos %}
import streamlit as st
from openai import OpenAI

st.set_page_config(
    page_title="Streamlit Example",
    page_icon="😁",
    layout="wide",
    initial_sidebar_state="expanded",
    menu_items={
        "About": "This is a example page.",
        "Get Help": "https://streamlit.io/",
    }
)

system_prompt = "You are a helpful assistant."

client = OpenAI(
    # Gemini 提供的 OpenAI 兼容 Base URL
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)

if "messages" not in st.session_state:
    st.session_state.messages = []

for message in st.session_state.messages:
    st.chat_message(message["role"]).write(message["content"])

prompt = st.chat_input("請問你要問什麼問題")
if prompt:
    st.chat_message("user").write(prompt)
    # 儲存user的prompt
    st.session_state.messages.append({"role": "user", "content": prompt})
    print([
        {"role": "system", "content": system_prompt},
        *st.session_state.messages
    ])
    response = client.chat.completions.create(
        # 使用 Gemini 的模型名稱
        model="gemini-3-flash-preview",
        messages=[
            {"role": "system", "content": system_prompt},
            *st.session_state.messages
        ],
        stream=True  # 串流輸出, 注意是大寫的True
    )
    # 使用空容器
    response_message = st.empty()
    full_response = ""
    for chunk in response:
        if chunk.choices[0].delta.content:
            content = chunk.choices[0].delta.content
            full_response += content
            # 使用空容器
            response_message.chat_message("assistant").write(full_response)
    # 儲存ai回覆
    st.session_state.messages.append({"role": "assistant", "content": full_response})

{% endhighlight %}