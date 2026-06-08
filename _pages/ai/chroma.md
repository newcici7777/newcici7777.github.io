---
title: chroma
date: 2026-06-03
keywords: Langchain, Gemini, GoogleGenerativeAIEmbeddings, langchain-chroma, chromadb
---
## 安裝chroma
python3.14 無法安裝chroma，需要降級至python3.11。<br>
降級安裝方式，請參考下方連結最後面:<br>
- [python安裝][1]

固定要安裝的清單
```
pip3 install langchain
pip3 install langchain-core
pip3 install langchain-community   
pip3 install langchain-google-genai
```

chroma 要安裝的清單
```
pip3 install langchain-chroma
pip3 install chromadb
```

info.csv
```
action
請假
休假
工作
讀書
旅行
```

## 增加Chroma程式碼

{% highlight python linenos %}
from langchain_community.document_loaders import CSVLoader
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_google_genai import GoogleGenerativeAIEmbeddings
from langchain_chroma import Chroma
embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")

# Chroma程式碼
vector_store = Chroma(
    collection_name="test", # db 名字
    embedding_function= embeddings,
    persist_directory="./chroma_db" # db儲存路徑
) 

# 沒有設欄位名，預設以第一列action作為欄位名
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
以下會產生三筆Document，`id='id1'`為程式碼給的id編號，page_content 為action:符合詢問的條件，row為資料是第幾筆，資料從action之後的列數開始數，「請假」的index為0, 最後一筆「旅行」的index為4。
```
[Document(id='id1', metadata={'row': 1, 'source': './data/info.csv'}, page_content='action: 休假'), Document(id='id4', metadata={'source': './data/info.csv', 'row': 4}, page_content='action: 旅行'), Document(id='id0', metadata={'row': 0, 'source': './data/info.csv'}, page_content='action: 請假')]
```

## 把CSVLoader註解
{% highlight python linenos %}
from langchain_community.document_loaders import CSVLoader
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_google_genai import GoogleGenerativeAIEmbeddings
from langchain_chroma import Chroma
embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")

vector_store = Chroma(
    collection_name="test",
    embedding_function= embeddings,
    persist_directory="./chroma_db")

# 以下內容註解仍能執行
# loader = CSVLoader(
#     file_path="./data/info.csv",
# )
#
# documents = loader.load()
# vector_store.add_documents(
#     documents=documents,
#     ids=["id"+str(i) for i in range(len(documents))],
# )
result = vector_store.similarity_search(
    "我想出去玩",
    3
)
print(result)
{% endhighlight %}
```
[Document(id='id1', metadata={'source': './data/info.csv', 'row': 1}, page_content='action: 休假'), Document(id='id4', metadata={'row': 4, 'source': './data/info.csv'}, page_content='action: 旅行'), Document(id='id0', metadata={'source': './data/info.csv', 'row': 0}, page_content='action: 請假')]
```



[1]: {% link _pages/python/install.md %}