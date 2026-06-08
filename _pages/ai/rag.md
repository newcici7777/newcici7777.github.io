---
title: RAG 實作
date: 2026-06-08
keywords: Langchain, Gemini, RAG
---
```
pip3 install streamlit
```

{% highlight python linenos %}
import streamlit as st

st.title("Data Uploaded")
uploaded_file = st.file_uploader(
    "Upload a file",
    type=["txt"],
    accept_multiple_files=False, )
if uploaded_file is not None:
    file_name = uploaded_file.name
    file_type = uploaded_file.type
    file_size = uploaded_file.size / 1024
    st.subheader(f"文件名:{file_name}")
    st.write(f"格式:{file_type} | 大小:{file_size:.2f} KB")
    text = uploaded_file.getvalue().decode("utf-8")
    st.write(text)
{% endhighlight %}

```
md5_path = "./md5.text"

# Chroma
collection_name = "rag"
persist_directory = "./chroma_db2"

# spliter
chunk_size = 10
chunk_overlap = 0
separators = ["\n\n", "\n", "。", ".", "!"]
max_split_char_number = 1000
```

{% highlight python linenos %}
import os
import config_data as config
import hashlib
from langchain_google_genai import GoogleGenerativeAIEmbeddings, ChatGoogleGenerativeAI
from langchain_chroma import Chroma
from langchain_text_splitters import RecursiveCharacterTextSplitter


# 檢查是否存在md5檔案
def check_md5(md5_str: str):
    if not os.path.exists(config.md5_path):
        # 沒有檔案，就建立
        open(config.md5_path, 'w', encoding="utf-8").close()
        return False
    else:
        # 讀取每一行
        for line in open(config.md5_path, 'r', encoding="utf-8").readlines():
            line = line.strip()
            if line == md5_str:
                return True
        # 檔案沒有資料傳回false
        return False


# 寫入md5
def save_md5(md5_str: str):
    with open(config.md5_path, 'a', encoding="utf-8") as f:
        f.write(md5_str + '\n')


# 取得md5
def get_string_md5(input_str: str, encoding="utf-8"):
    # 字串轉成byte
    str_bytes = input_str.encode(encoding)
    # 產生md5 物件
    md5_obj = hashlib.md5()
    # 將字串byte放入
    md5_obj.update(str_bytes)
    # 產生32bit 的數字(不管字串的長度多長)
    md5_hex = md5_obj.hexdigest()
    return md5_hex


class KnowledgeBaseService(object):
    def __init__(self):
        embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")
        self.chroma = Chroma(
            collection_name=config.collection_name,
            embedding_function=embeddings,
            persist_directory=config.persist_directory,
        )
        self.spliter = RecursiveCharacterTextSplitter(
            chunk_size=config.chunk_size,
            chunk_overlap=config.chunk_overlap,
            separators=config.separators,
            length_fuction=len,
        )


if __name__ == '__main__':
    # 建立檔案
    check_md5('cb92be636acc59e649b89668faf7008b')
    # 寫入檔案
    save_md5('cb92be636acc59e649b89668faf7008b')
    # 讀取檔案
    print(check_md5('cb92be636acc59e649b89668faf7008b'))

{% endhighlight %}