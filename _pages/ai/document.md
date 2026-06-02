---
title: CSVLoader, JsonLoader
date: 2026-06-02
keywords: Langchain, Gemini, CSVLoader, JsonLoader
---
## CSVLoader
student.csv
```
name,age,gender
Bill,10,1
Mary,15,0
Jello,12,1
Boka,11,0
```
{% highlight python linenos %}
from langchain_community.document_loaders.csv_loader import CSVLoader

loader = CSVLoader(
    file_path= "./data/student.csv",
    #csv_args={"delimiter":","},
    encoding="utf-8")
# documents = loader.load()
for document in loader.lazy_load():
    print(type(document), document)
{% endhighlight %}

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

### 取得所有key、value
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student0.json", # json檔案路徑
    jq_schema=".", # .為根目錄，代表顯示所有key value
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
{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student0.json", # json檔案路徑
    jq_schema=".name", # 顯示key為name
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
取得`hobby[1]`
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
以下結果，內容是在page_content，source為來源資料，seq_num為每一列的序號。<br>
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
以下結果，內容是在page_content，source為來源資料，seq_num為每一列的序號。<br>
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

## 結論

- json_lines=True 為每一行都是各別獨立的json
- text_content=False 內容為文字嗎？true代表是。

json
- .name 只取得key為name的value
- .info.tel 取得info子json中，key為tel的value

list:
- [] 取出list中所有元素
- [].name 取出list中所有元素，但key為name
- [1] 根據index，取出特定的元素