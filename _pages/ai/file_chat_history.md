---
title: FileChatMessageHistory
date: 2026-05-26
keywords: Langchain, Gemini, FileChatMessageHistory
---

{% highlight python linenos %}
import json
from typing import Sequence

from langchain_core.messages import message_to_dict, messages_from_dict, BaseMessage
from langchain_core.chat_history import BaseChatMessageHistory
import os

from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.runnables import RunnableWithMessageHistory
from langchain_google_genai import ChatGoogleGenerativeAI
from requests import session

class FileChatMessageHistory(BaseChatMessageHistory):
    def __init__(self, session_id, storage_path):
        self.session_id = session_id
        self.storage_path = storage_path
        self.file_path =os.path.join(self.storage_path, self.session_id)
        os.makedirs(os.path.dirname(self.file_path), exist_ok=True)

    def add_messages(self, messages: Sequence[BaseMessage]) -> None:
        all_messages = list(self.messages)
        all_messages.extend(messages)
        new_messages = [message_to_dict(message) for message in all_messages]
        print(new_messages)
        with open(self.file_path, 'w', encoding="utf-8") as f:
            json.dump(new_messages, f)

    @property
    def messages(self) -> list[BaseMessage]:
        try:
            with open(self.file_path, 'r', encoding="utf-8") as f:
                messages_data = json.loads(f.read())
                return messages_from_dict(messages_data)
        except FileNotFoundError:
            return []

    def clear(self) -> None:
        with open(self.file_path, 'w', encoding="utf-8") as f:
            json.dump([], f)

# 初始化 Gemini LLM
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)

prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "你需要根據聊天記錄回應使用者問題。聊天記錄:"),
        MessagesPlaceholder("chat_history"),
        ("human", "請回答如下問題:{input}")
    ]
)

str_parser = StrOutputParser()

def print_prompt(full_prompt):
    print("=" * 20, full_prompt.to_string(), "=" * 20)
    return full_prompt

base_chain = prompt | print_prompt | llm | str_parser

def get_history(session_id):
    return FileChatMessageHistory(session_id, "./chat_history")

conversation_chain = RunnableWithMessageHistory(
    base_chain,
    get_history,
    input_messages_key="input",
    history_messages_key="chat_history",
)

if __name__ == "__main__":
    session_config = {
        "configurable":{
            "session_id":"user_001"
        }
    }
    # res = conversation_chain.invoke({"input":"小明有2隻貓"},session_config)
    # print("第1次執行:",res)
    # res = conversation_chain.invoke({"input":"小美有1隻狗"},session_config)
    # print("第2次執行:",res)
    res = conversation_chain.invoke({"input":"總共幾隻寵物"},session_config)
    print(res)
{% endhighlight %}