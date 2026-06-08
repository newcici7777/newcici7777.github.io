---
title: 向量資料庫（Vector Database）
date: 2026-06-03
keywords: Langchain, Gemini, GoogleGenerativeAIEmbeddings, InMemoryVectorStore, Vector store
---
以下的csv檔案
info.csv
```
action
請假
休假
工作
讀書
旅行
```
第一列欄位名是action，索引index 0從請假開始，旅行的index為4。<br>
```
欄位名   action
index=0 請假
index=1 休假
index=2 工作
index=3 讀書
index=4 旅行
```

## add_documents()
add_documents()把document增加至向量資料庫。<br>

一筆一筆新增語法:
```
vector_store.add_documents(documents = [文件1, 文件2], ids = [編號1, 編號2])
vector_store.add_documents(documents = [document1, document2], ids = [id1, id2])
```

多筆新增語法:
```
# 取得多筆documents，回傳值為list[document1, document2, document3 ..]
documents = loader.load()

vector_store.add_documents(
    documents = 文件list,
    ids = 根據文件的大小產生序號,
)

vector_store.add_documents(
    documents = documents,
    ids = ["id"+str(i) for i in range(len(documents))],
)
```

```
 ids=["id"+str(i) for i in range(len(documents))],
```
以上程式碼，會組成以下的id編號在資料庫，每一個id編號，都對映csv中的一個document。
```
id0 請假
id1 休假
id2 工作
id3 讀書
id4 旅行
```
## similarity_search
similarity_search 在向量資料庫搜尋比對的內容。
```
result = vector_store.similarity_search(
    要搜尋比對的內容,
    要比對的數量
)

result = vector_store.similarity_search(
    "我想出去玩",
    3
)
```

完整程式碼
{% highlight python linenos %}
from langchain_community.document_loaders import CSVLoader
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_google_genai import GoogleGenerativeAIEmbeddings

embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")

vector_store = InMemoryVectorStore(embedding=embeddings)
loader = CSVLoader(
    file_path="./data/info.csv",
)
documents = loader.load()
vector_store.add_documents(
    documents=documents,
    ids=["id"+str(i) for i in range(len(documents))],
)
result = vector_store.similarity_search(
    "我想出去玩",
    3
)
print(result)
{% endhighlight %}
以下會產生三筆Document，page_content 為欄名action:符合詢問的條件，row為資料是第幾筆，資料從action之後的列數開始數，「請假」的index為0, 最後一筆「旅行」的index為4。<br>

比對到三筆跟「我想出去玩」有相關的內容有:「休假」、「旅行」、「請假」<br>
```
[Document(id='id1', metadata={'source': './data/info.csv', 'row': 1}, page_content='action: 休假'), Document(id='id4', metadata={'source': './data/info.csv', 'row': 4}, page_content='action: 旅行'), Document(id='id0', metadata={'source': './data/info.csv', 'row': 0}, page_content='action: 請假')]
```

## delete 刪除向量資料
```
vector_store.delete(ids = ["id1", "id2"])
```

## add_texts()
add_texts()使用字串list作為向量資料庫內容，會自動產生id。
{% highlight python linenos %}
from langchain_community.document_loaders import CSVLoader
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_google_genai import GoogleGenerativeAIEmbeddings

embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")

vector_store = InMemoryVectorStore(embedding=embeddings)
vector_store.add_texts(["減肥就是少吃多動", "鬼滅之刃真好看", "跑步是很好的運動", "節制食量，控制飲食", "早睡早起", "炭治郎真帥"])
result = vector_store.similarity_search(
    "如何減肥",
    3
)
print(result)
{% endhighlight %}
```
[Document(id='c2e550ca-3a19-47fc-aa21-008aed0aad3d', metadata={}, page_content='減肥就是少吃多動'), Document(id='4ada8884-936a-4615-989e-1c8f78b8461e', metadata={}, page_content='節制食量，控制飲食'), Document(id='c9b69375-40ec-4131-9032-b5c121f460c2', metadata={}, page_content='跑步是很好的運動')]
```

用for 取得 page_content
{% highlight python linenos %}
from langchain_community.document_loaders import CSVLoader
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_google_genai import GoogleGenerativeAIEmbeddings

embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")

vector_store = InMemoryVectorStore(embedding=embeddings)
vector_store.add_texts(["減肥就是少吃多動", "鬼滅之刃真好看", "跑步是很好的運動", "節制食量，控制飲食", "早睡早起", "炭治郎真帥"])
result = vector_store.similarity_search(
    "如何減肥",
    3
)
for doc in result:
    print(doc.page_content)
{% endhighlight %}
```
減肥就是少吃多動
節制食量，控制飲食
跑步是很好的運動
```

與模型結合，讓使用者提出問題，但參考資料以向量資料庫為主。
{% highlight python linenos %}
from langchain_community.document_loaders import CSVLoader
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_google_genai import GoogleGenerativeAIEmbeddings, ChatGoogleGenerativeAI

embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")
vector_store = InMemoryVectorStore(embedding=embeddings)
vector_store.add_texts(
    ["減肥就是少吃多動", "鬼滅之刃真好看", "跑步是很好的運動", "節制食量，控制飲食", "早睡早起", "炭治郎真帥"])
input_text = "如何減肥"
result = vector_store.similarity_search(
    input_text,
    3
)
reference_text = "["
for doc in result:
    reference_text += doc.page_content
reference_text += "]"
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


chain = prompt | print_prompt | model | StrOutputParser()
res = chain.invoke({"input": input_text, "content": reference_text})
print(res)
{% endhighlight %}
```
System: 以我提供的參考資料為主，簡短的回答使用者問題。參考資料:[減肥就是少吃多動節制食量，控制飲食跑步是很好的運動]
Human: 使用者提出問題: 如何減肥
********************
減肥的核心在於「少吃多動」。具體做法包括節制食量、控制飲食，並配合跑步這類良好的運動。
```