---
title: CSVLoader, JsonLoader
date: 2026-06-02
keywords: Langchain, Gemini, CSVLoader, JsonLoader
---
## CSVLoader

![img]({{site.imgurl}}/ai/csv.png)

student.csv
```
name,age,gender
Bill,10,1
Mary,15,0
Jello,12,1
Boka,11,0
```

讀取documents有二種方法
```
loader.load()
loader.lazy_load()
```
{% highlight python linenos %}
from langchain_community.document_loaders.csv_loader import CSVLoader

loader = CSVLoader(
    file_path= "./data/student.csv",
    csv_args={"delimiter":","}, # 分隔符號
    encoding="utf-8")
# documents = loader.load()
for document in loader.lazy_load():
    print(type(document))
    print(document)
{% endhighlight %}
以下，每一列都是一個document，page_content則是每一列的內容，source則是來源檔案，row則是index索引。<br>
```
<class 'langchain_core.documents.base.Document'>
page_content='name: Bill
age: 10
gender: 1' metadata={'source': './data/student.csv', 'row': 0}
<class 'langchain_core.documents.base.Document'>
page_content='name: Mary
age: 15
gender: 0' metadata={'source': './data/student.csv', 'row': 1}
<class 'langchain_core.documents.base.Document'>
page_content='name: Jello
age: 12
gender: 1' metadata={'source': './data/student.csv', 'row': 2}
<class 'langchain_core.documents.base.Document'>
page_content='name: Boka
age: 11
gender: 0' metadata={'source': './data/student.csv', 'row': 3}
```

## JsonLoader
### 安裝jq
```
pip3 install jq    
```
### json
以下結構， name 是 string ， age 是 int ， hobby 是 list ， info 是 json 。<br>
student0.json<br>
```
{
  "name": "Mary",
  "age": 10,
  "gender": 0,
  "hobby": [
    "singing",
    "swimming",
    "cooking"
  ],
  "info": {
    "tel": "03-111111",
    "address": "Taiwan, Taoyuan City"
  }
}
```
### JSONLoader 參數
- json_lines=True 為每一行都是各別獨立的json
- text_content=False 內容為文字嗎？true代表是。

### jq_schema
json
- .name 只取得key為name的value
- .info.tel 取得info子json中，key為tel的value

list:
- [] 取出list中所有元素
- [].name 取出list中所有元素，但key為name
- [1] 根據index，取出特定的元素

### 取得所有key、value
. 為根目錄，代表顯示所有key的 value
```
jq_schema="."
```
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student0.json", # json檔案路徑
    jq_schema=".", # .為根目錄，代表顯示所有key的 value
    text_content= False
)
documents = loader.load()
print(documents)
{% endhighlight %}
以下，內容是在page_content，source為來源資料，seq_num為每一列的序號。<br>
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student0.json', 'seq_num': 1}, page_content='{"name": "Mary", "age": 10, "gender": 0, "hobby": ["singing", "swimming", "cooking"], "info": {"tel": "03-111111", "address": "Taiwan, Taoyuan City"}}')]
```

### 取得特定key的value
顯示key為name的 value
```
jq_schema = ".name"
```
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student0.json", # json檔案路徑
    jq_schema=".name", # 顯示key為name的value
    text_content= False
)
documents = loader.load()
print(documents)
{% endhighlight %}
以下，內容是在page_content，只顯示名字，source為來源資料，seq_num為每一列的序號。<br>
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student0.json', 'seq_num': 1}, page_content='Mary')]
```

### 透過子元素key，取出value
key為info，再取得子json中key為tel
```
jq_schema=".info.tel"
```
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student0.json", # json檔案路徑
    jq_schema=".info.tel", # key為info，再取得子json中key為tel
    text_content= False
)
documents = loader.load()
print(documents)
{% endhighlight %}
以下，內容是在page_content，只顯示電話，source為來源資料，seq_num為每一列的序號。<br>
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student0.json', 'seq_num': 1}, page_content='03-111111')]
```

### 取得 list 所有元素
取得key為hobby，`[]`取得所有list
```
jq_schema=".hobby[]"
```
取得所有hobby list，每一個元素都是Document物件，所以結果會有三個Document物件，顯示的資料在page_content。
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student0.json", # json檔案路徑
    jq_schema=".hobby[]", # []代表取得所有list
    text_content= False
)
documents = loader.load()
print(documents)
{% endhighlight %}
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student0.json', 'seq_num': 1}, page_content='singing'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student0.json', 'seq_num': 2}, page_content='swimming'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student0.json', 'seq_num': 3}, page_content='cooking')]
```

### 根據index 索引，取得元素
取得`hobby[1]`根據索引1取得元素內容。
```
jq_schema=".hobby[1]"
```
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student0.json", # json檔案路徑
    jq_schema=".hobby[1]", # [index] 根據索引取得元素 
    text_content= False
)
documents = loader.load()
print(documents)
{% endhighlight %}
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student0.json', 'seq_num': 1}, page_content='swimming')]
```

## 每一行都是一個json元素
student.json
```
{"name": "Mary", "age": 10, "gender": 0}
{"name": "Bill", "age": 11, "gender": 1}
{"name": "Jello", "age": 10, "gender": 0}
```

### json_lines 設為 True
每一行為獨立的json
```
json_lines=True
```
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student.json", # json檔案路徑
    jq_schema=".",  # 從根目錄
    text_content=False, # 內容是文字嗎? 
	json_lines=True # 每一行為獨立的json
)
documents = loader.load()
print(documents)
{% endhighlight %}
以下有三個document元素，內容是在page_content，source為來源資料，seq_num為每一列的序號。<br>
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 1}, page_content='{"name": "Mary", "age": 10, "gender": 0}'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 2}, page_content='{"name": "Bill", "age": 11, "gender": 1}'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 3}, page_content='{"name": "Jello", "age": 10, "gender": 0}')]
```

### 只取得key為name
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student.json", # json檔案路徑
    jq_schema=".name",  # 只取key為name的資料
    text_content=False, # 內容是文字嗎?
	json_lines=True # 每一行為獨立的json檔案
)
documents = loader.load()
print(documents)
{% endhighlight %}
以下有三個document元素，內容是在page_content，source為來源資料，seq_num為每一列的序號。<br>
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 1}, page_content='Mary'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 2}, page_content='Bill'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 3}, page_content='Jello')]
```

## list中的多個json
student2.json，json資料放在list中。<br>
```
[
  {"name": "Mary", "age": 10, "gender": 0},
  {"name": "Bill", "age": 11, "gender": 1},
  {"name": "Jello", "age": 10, "gender": 0}
]
```
### 取得list中json的key為name
`[]`代表取得所有list，`.name`代表只取得key為name的value。
```
jq_schema=".[].name"
```
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student2.json",
    jq_schema=".[].name",
    text_content= False
)
documents = loader.load()
print(documents)
{% endhighlight %}
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student2.json', 'seq_num': 1}, page_content='Mary'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student2.json', 'seq_num': 2}, page_content='Bill'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student2.json', 'seq_num': 3}, page_content='Jello')]
```

## TextLoader
```
天氣好
你好
Hello!
Nice to meet you!
```

{% highlight python linenos %}
from langchain_community.document_loaders import TextLoader
from langchain_text_splitters import RecursiveCharacterTextSplitter

loader = TextLoader(
    "./data/content.txt",
    encoding="utf-8",
)
docs = loader.load()
splitter = RecursiveCharacterTextSplitter(
    chunk_size=10,  # 每一個段落最大的字元總數
    chunk_overlap=0,  # 每一個段落允許重覆的字元數量
    # 段落切割的依據
    separators=["\n\n", "\n", "。", ".", "!"],
    length_function=len,  # 計算字元數量用python的len函式
)

split_docs = splitter.split_documents(docs)
print(len(split_docs))
for doc in split_docs:
    print("*" * 20)
    print(doc)
{% endhighlight %}
```
3
********************
page_content='Hello!' metadata={'source': './data/content.txt'}
********************
page_content='
Nice to meet you' metadata={'source': './data/content.txt'}
********************
page_content='!' metadata={'source': './data/content.txt'}
```

## PyPDFLoader
```
pip3 install pypdf
``` 
語法
```
loader = PyPDFLoader(
    file_path="./data/trip.pdf",
    mode="page", # page為不同頁面是一個document, single為整個檔案是一個document
    password="", # 如果檔案有密碼
)
```
{% highlight python linenos %}
from langchain_community.document_loaders import PyPDFLoader

loader = PyPDFLoader(
    file_path="./data/trip.pdf",
    mode="page", # page為不同頁面是一個document, single為整個檔案是一個document
)
for doc in loader.lazy_load():
    print(doc)
{% endhighlight %}
page_content是內容，page_label為頁碼
```
page_content='' metadata={'producer': 'Skia/PDF m147 Google Docs Renderer', 'creator': 'PyPDF', 'creationdate': '', 'title': '未命名文件', 'source': './data/trip.pdf', 'total_pages': 3, 'page': 0, 'page_label': '1'}
page_content='' metadata={'producer': 'Skia/PDF m147 Google Docs Renderer', 'creator': 'PyPDF', 'creationdate': '', 'title': '未命名文件', 'source': './data/trip.pdf', 'total_pages': 3, 'page': 1, 'page_label': '2'}
page_content='' metadata={'producer': 'Skia/PDF m147 Google Docs Renderer', 'creator': 'PyPDF', 'creationdate': '', 'title': '未命名文件', 'source': './data/trip.pdf', 'total_pages': 3, 'page': 2, 'page_label': '3'}
```