---
title: 向量資料庫（Vector Database）
date: 2026-06-03
keywords: Langchain, Gemini, GoogleGenerativeAIEmbeddings, InMemoryVectorStore, Vector store
---
info.csv
```
請假
休假
旅行
工作
讀書
考試
做家事
```
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
```
[Document(id='id0', metadata={'source': './data/info.csv', 'row': 0}, page_content='請假: 休假')]
```