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
安裝jq
```
pip3 install jq    
```
student0.json<br>
以下結構， name 是 string ， age 是 int ， hobby 是 list ， info 是 json 。
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


student.json
```
{"name": "Mary", "age": 10, "gender": 0}
{"name": "Bill", "age": 11, "gender": 1}
{"name": "Jello", "age": 10, "gender": 0}
```

{% highlight python linenos %}
from langchain_community.document_loaders import JSONLoader

loader = JSONLoader(
    file_path= "./data/student.json", # json檔案路徑
    jq_schema=".",  # 從根目錄
    text_content=False, # 內容是文字嗎? 
	json_lines=True # 每一行為獨立的json檔案
)
documents = loader.load()
print(documents)
{% endhighlight %}
以下結果，內容是在page_content，source為來源資料，seq_num為每一列的序號。<br>
```
[Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 1}, page_content='{"name": "Mary", "age": 10, "gender": 0}'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 2}, page_content='{"name": "Bill", "age": 11, "gender": 1}'), Document(metadata={'source': '/Users/cici/PythonProject/AIProject/data/student.json', 'seq_num': 3}, page_content='{"name": "Jello", "age": 10, "gender": 0}')]
```

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

student2.json，json資料放在list中。<br>
```
[
  {"name": "Mary", "age": 10, "gender": 0},
  {"name": "Bill", "age": 11, "gender": 1},
  {"name": "Jello", "age": 10, "gender": 0}
]
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
