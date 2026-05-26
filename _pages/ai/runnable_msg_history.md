---
title: RunnableWithMessageHistory
date: 2026-05-26
keywords: Langchain, Gemini, RunnableWithMessageHistory
---
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
    res = conversation_chain.invoke("小明有2隻貓",session_config)
    res = conversation_chain.invoke("小花有1隻狗",session_config)
    res = conversation_chain.invoke("總共有幾隻寵物",session_config)
    print(res)
{% endhighlight %}


以下是官網範例<br>
<https://reference.langchain.com/python/langchain-core/runnables/history/RunnableWithMessageHistory><br>

{% highlight python linenos %}
from typing import Optional

from langchain_google_genai import ChatGoogleGenerativeAI

# from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
# from langchain_core.runnables.history import RunnableWithMessageHistory
##############
from langchain_core.chat_history import BaseChatMessageHistory
from langchain_core.documents import Document
from langchain_core.messages import BaseMessage, AIMessage
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from pydantic import BaseModel, Field
from langchain_core.runnables import (
    RunnableLambda,
    ConfigurableFieldSpec,
    RunnablePassthrough,
)
from langchain_core.runnables.history import RunnableWithMessageHistory
class InMemoryHistory(BaseChatMessageHistory, BaseModel):
    """In memory implementation of chat message history."""

    messages: list[BaseMessage] = Field(default_factory=list)

    def add_messages(self, messages: list[BaseMessage]) -> None:
        """Add a list of messages to the store"""
        self.messages.extend(messages)

    def clear(self) -> None:
        self.messages = []

# Here we use a global variable to store the chat message history.
# This will make it easier to inspect it to see the underlying results.
store = {}

def get_by_session_id(session_id: str) -> BaseChatMessageHistory:
    if session_id not in store:
        store[session_id] = InMemoryHistory()
    return store[session_id]

history = get_by_session_id("1")
history.add_message(AIMessage(content="hello"))
################




prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "You're an assistant who's good at {ability}"),
        MessagesPlaceholder(variable_name="history"),
        ("human", "{question}"),
    ]
)
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
chain = prompt | llm

chain_with_history = RunnableWithMessageHistory(
    chain,
    # Uses the get_by_session_id function defined in the example
    # above.
    get_by_session_id,
    input_messages_key="question",
    history_messages_key="history",
)

print(
    chain_with_history.invoke(  # noqa: T201
        {"ability": "math", "question": "What does cosine mean?"},
        config={"configurable": {"session_id": "foo"}},
    )
)

# Uses the store defined in the example above.
print(store)  # noqa: T201

print(
    chain_with_history.invoke(  # noqa: T201
        {"ability": "math", "question": "What's its inverse"},
        config={"configurable": {"session_id": "foo"}},
    )
)

print(store)  # noqa: T201
{% endhighlight %}