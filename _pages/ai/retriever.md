---
title: as_retriever()
date: 2026-06-08
keywords: Langchain, Gemini, GoogleGenerativeAIEmbeddings, InMemoryVectorStore, Vector store,as_retriever
---
語法
```
vector_store.as_retriever(search_kwargs={"k": 要比對的數量})
vector_store.as_retriever(search_kwargs={"k": 2})
```

prompt 的輸入參數有二個，分別是content, input。
```
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "以我提供的參考資料為主，簡短的回答使用者問題。參考資料:{content}"),
        ("user", "使用者提出問題: {input}")
    ]
)
```

下方呼叫chain，是把input_text的字串「如何減肥」，作為prompt 的輸入參數input。<br>
```
input_text = "如何減肥"
res = chain.invoke(input_text)
```

RunnablePassthrough()會自動把`input_text`轉成prompt 的輸入參數input
```
{"input": RunnablePassthrough(),
         "content": retriever | format_func}
```
retriever的傳回值是list[documents]，因為list 沒有`__or__`方法，不能加入chain，所以要把list 轉成 string，透過format_func()函式轉成string之後，就可以把retriever的結果作為參數content 傳入 prompt。
{% highlight python linenos %}
def format_func(docs: list[Document]):
    if not docs:
        return "no docs"
    references = "["
    for doc in docs:
        references += doc.page_content
        references += ", "
    references += "]"
    return references
{% endhighlight %}

完整程式碼:
{% highlight python linenos %}
from langchain_community.document_loaders import CSVLoader
from langchain_core.documents import Document
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_google_genai import GoogleGenerativeAIEmbeddings, ChatGoogleGenerativeAI

embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")
vector_store = InMemoryVectorStore(embedding=embeddings)
vector_store.add_texts(
    ["減肥就是少吃多動", "鬼滅之刃真好看", "跑步是很好的運動", "節制食量，控制飲食", "早睡早起", "炭治郎真帥"])
input_text = "如何減肥"

retriever = vector_store.as_retriever(search_kwargs={"k": 2})

model = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "以我提供的參考資料為主，簡短的回答使用者問題。參考資料:{content}"),
        ("user", "使用者提出問題: {input}")
    ]
)


def print_prompt(prompt):
    print(prompt.to_string())
    print("*" * 20)
    return prompt


def format_func(docs: list[Document]):
    if not docs:
        return "no docs"
    references = "["
    for doc in docs:
        references += doc.page_content
        references += ", "
    references += "]"
    return references

chain = {"input": RunnablePassthrough(),
         "content": retriever | format_func} | prompt | print_prompt | model | StrOutputParser()
res = chain.invoke(input_text)
print(res)
{% endhighlight %}
```
System: 以我提供的參考資料為主，簡短的回答使用者問題。參考資料:[減肥就是少吃多動, 節制食量，控制飲食, ]
Human: 使用者提出問題: 如何減肥
********************
減肥的核心在於「少吃多動」，具體做法包括節制食量並嚴格控制飲食。
```