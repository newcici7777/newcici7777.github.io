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

## sidebar
![img]({{site.imgurl}}/streamlit/side_bar1.png)

簡單語法
{% highlight python linenos %}
st.sidebar.subheader("設定AI")
nick_name = st.sidebar.text_input("AI名字")
{% endhighlight %}

進階語法
{% highlight python linenos %}
with st.sidebar:
    st.subheader("設定AI")
    nick_name = st.sidebar.text_input("AI名字")
{% endhighlight %}

placeholder 與 value
{% highlight python linenos %}
with st.sidebar:
    st.subheader("設定AI")
    nick_name = st.sidebar.text_input(
        "AI名字",
        placeholder="AI名字", 
        value="王大明")
{% endhighlight %}

![img]({{site.imgurl}}/streamlit/ai_setting.png)<br>

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

client = OpenAI(
    # Gemini 提供的 OpenAI 兼容 Base URL
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)

if "messages" not in st.session_state:
    st.session_state.messages = []

# 增加AI名字
if "nick_name" not in st.session_state:
    st.session_state.nick_name = ""

# 增加AI個性
if "personality" not in st.session_state:
    st.session_state.personality = ""

# 增加側邊欄
with st.sidebar:
	# 標題
    st.subheader("設定AI")
    
    # AI名字
    nick_name = st.sidebar.text_input(
        "AI名字",
        placeholder="AI名字", value= st.session_state.nick_name)
    if nick_name:
        st.session_state.nick_name = nick_name

    # AI個性
    personality = st.sidebar.text_input(
        "AI的個性",
        placeholder="AI的個性", value = st.session_state.personality)
    if personality:
        st.session_state.personality = personality

# prompt 設定
system_prompt = "你是一個得力助手"
if nick_name and personality:
    system_prompt = """
    你的名字是:%s, 你是客戶的好朋友
    你的個性是:%s
    規則:
    1. 每次只回1條消息
    2. 根據客戶的語言，回應相同語言
    3. 根據設定個性，回覆客戶問題。
    """ % (st.session_state.nick_name,st.session_state.personality)

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
            {"role": "system", "content": system_prompt },
            *st.session_state.messages
        ],
        stream=True  # 串流輸出, 注意是大寫的True
    )
    response_message = st.empty()
    full_response = ""
    for chunk in response:
        if chunk.choices[0].delta.content:
            content = chunk.choices[0].delta.content
            full_response += content
            response_message.chat_message("assistant").write(full_response)
    # 儲存ai回覆
    st.session_state.messages.append({"role": "assistant", "content": full_response})

{% endhighlight %}

## 完整程式碼
![img]({{site.imgurl}}/streamlit/chat_msg3.png)<br>

{% highlight python linenos %}
import os
import json
import streamlit as st
from openai import OpenAI
from datetime import datetime

# 取得所有json檔案
def load_sessions():
    session_list = []
    # 判斷是否有sessions目錄
    if os.path.exists("sessions"):
        # 列出sessions目錄下所有檔案
        file_list = os.listdir("sessions")
        # 迴圈
        for filename in file_list:
            # 只抓取副檔名是.json
            if filename.endswith(".json"):
                # -5 從後面數
                session_list.append(filename[:-5])
    # 把list排序並翻轉，日期最新在最上面
    session_list.sort(reverse=True)
    return session_list

# 讀取單個對話記錄
def load_session(session_name):
    try:
        # 判斷檔案是否存在
        if os.path.exists(f"sessions/{session_name}.json"):
            # 使用with打開檔案
            with open(f"sessions/{session_name}.json", "r", encoding="utf-8") as f:
                # 把json內容轉成dict
                session_data = json.load(f)
                # 把dict內容 分別塞入 session
                st.session_state.messages = session_data["messages"]
                st.session_state.nick_name = session_data["nick_name"]
                st.session_state.personality = session_data["personality"]
                st.session_state.current_session = session_name
    except Exception as e:
        st.error("歷史對話加載失敗")

# 儲存對話記錄
def save_session():
    # 目前的session不為空
    if st.session_state.current_session:
        # 建立dict物件
        session_data = {
            "nick_name": st.session_state.nick_name,
            "personality": st.session_state.personality,
            "current_session": st.session_state.current_session,
            "messages": st.session_state.messages,
        }
        # 判斷sessions目錄是否存在
        if not os.path.exists("sessions"):
            os.mkdir("sessions")
        # 使用 with 寫入檔案
        with open(f"sessions/{st.session_state.current_session}.json", "w", encoding="utf-8") as f:
            # 將dict轉成json格式寫入檔案
            json.dump(session_data, f)

# 刪除對話記錄
def delete_session(session_name):
    try:
        # 判斷檔案存在
        if os.path.exists(f"sessions/{session_name}.json"):
            # 刪除檔案
            os.remove(f"sessions/{session_name}.json")
            # 若刪除的檔案與 目前網頁右側對話記錄是同一個
            if session_name == st.session_state.current_session:
                # 清空
                st.session_state.messages = []
                # 產生新的對話日期
                st.session_state.current_session = generate_session_name()
    except Exception:
        st.error("刪除對話記錄失敗")

# 根據年月日時分秒產生 session name
def generate_session_name():
    return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

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

client = OpenAI(
    # Gemini 提供的 OpenAI 兼容 Base URL
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/"
)
# session_state 建立 key
if "messages" not in st.session_state:
    st.session_state.messages = []

if "nick_name" not in st.session_state:
    st.session_state.nick_name = ""

if "personality" not in st.session_state:
    st.session_state.personality = ""

if "current_session" not in st.session_state:
    # 取得session name
    now = generate_session_name()
    st.session_state.current_session = now

# 側邊欄
with st.sidebar:
    st.subheader("設定AI")
    # stretch代表與父物件的寬度一樣
    if st.button("建立新對話", width="stretch"):
        # 儲存對話記錄
        save_session()
        # 建立新的對話記錄
        if st.session_state.messages:
            st.session_state.messages = []
            # 取得session name
            st.session_state.current_session = generate_session_name()
            # 儲存對話記錄
            save_session()
            # 重新執行
            st.rerun()
    # 歷史對話
    st.text(f"歷史對話:{st.session_state.current_session}")
    # 取得所有json檔案
    session_list = load_sessions()
    # 迴圈
    for session in session_list:
        # 設定2個欄位，分別是col1 col2
        # 欄位比例設定，總共5等份，col1為4等份寬，col2 1等份寬
        col1, col2 = st.columns([4,1])
        # col1 內容
        with col1:
            # 讀取對話記錄
            if st.button(session, width="stretch", icon="📃", key=f"load_{session}",
                         type = "primary" if session == st.session_state.current_session else "secondary"):
                # 讀取對話記錄
                load_session(session)
                # 重新執行
                st.rerun()
        # col2 內容
        with col2:
            # 刪除
            if st.button("", width="stretch", icon ="❌", key=f"delete_{session}"):
                # 刪除對話記錄
                delete_session(session)
                # 重新執行
                st.rerun()
    # 分隔線
    st.divider()

    # AI名字輸入框
    nick_name = st.sidebar.text_input(
        "AI名字",
        placeholder="AI名字", value= st.session_state.nick_name)
    if nick_name:
        # 設定session
        st.session_state.nick_name = nick_name

    # AI個性輸入框
    personality = st.sidebar.text_input(
        "AI的個性",
        placeholder="AI的個性", value = st.session_state.personality)
    if personality:
        # 設定session
        st.session_state.personality = personality

# 預設prompt
system_prompt = "你是一個得力助手"
# 若AI名字與個性有設定，使用設定
if nick_name and personality:
    system_prompt = """
    你的名字是:%s, 你是客戶的好朋友
    你的個性是:%s
    規則:
    1. 每次只回1條消息
    2. 根據客戶的語言，回應相同語言
    3. 根據設定個性，回覆客戶問題。
    """ % (st.session_state.nick_name,st.session_state.personality)

# 列出目前session的對話記錄
for message in st.session_state.messages:
    st.chat_message(message["role"]).write(message["content"])
# 輸入框
prompt = st.chat_input("請問你要問什麼問題")
if prompt:
    # 設定對話內容的身份為使用者，prompt為提問的內容
    st.chat_message("user").write(prompt)
    # 儲存user的prompt
    st.session_state.messages.append({"role": "user", "content": prompt})
    # 問AI
    response = client.chat.completions.create(
        # 使用 Gemini 的模型名稱
        model="gemini-3-flash-preview",
        messages=[
            {"role": "system", "content": system_prompt }, #AI的系統設定
            *st.session_state.messages #上下文對話記錄
        ],
        stream=True  # 串流輸出, 注意是大寫的True
    )
    # 空對話
    response_message = st.empty()

    # 串流輸出要用到的程式碼
    full_response = ""
    for chunk in response:
        # 判斷內容是否為空
        if chunk.choices[0].delta.content:
            # 取得內容
            content = chunk.choices[0].delta.content
            full_response += content
            # 設定回覆對話的身份為AI，full_response為回覆的內容
            response_message.chat_message("assistant").write(full_response)
    # 儲存ai回覆
    st.session_state.messages.append({"role": "assistant", "content": full_response})

    # 儲存對話記錄
    save_session()
{% endhighlight %}
