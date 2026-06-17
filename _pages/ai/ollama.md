---
title: Ollama
date: 2026-06-17
keywords: Ollama
---
舊版MAC安裝Ollama步驟。

下載真正相容舊 Intel 的 0.1.48 核心與副核心
回到你目前這個分頁，執行這行指令。我們這次直接從 GitHub 把相容的 Mac Intel 壓縮包抓下來：
```
cd ~/Downloads
# 下載 v0.1.48 的 Mac 完整包
curl -L "https://github.com/ollama/ollama/releases/download/v0.1.48/Ollama-darwin.zip" -o Ollama-v0148.zip
# 解壓到專屬資料夾
unzip Ollama-v0148.zip -d Ollama_Old
```

替換主核心與副核心
我們把舊版相容的器官通通換上去：
```
# 覆蓋主核心
sudo cp Ollama_Old/Ollama.app/Contents/Resources/ollama /usr/local/bin/ollama
sudo chmod +x /usr/local/bin/ollama

# 覆蓋副核心（舊版的副核心結構不同，我們直接用 find 抓出來放進去）
sudo find Ollama_Old/Ollama.app -name "llama-server" -exec cp {} /usr/local/lib/ollama/ \;
sudo chmod +x /usr/local/lib/ollama/llama-server
```

執行ollama
```
Bash
ollama serve
```

下載模型
```
# 下載 Google 推出、支援舊版相容的輕量模型
ollama pull gemma:2b

# 或者下載對中文支援極好、只有 1.8B 的模型
ollama pull qwen:1.8b
```

直接在終端機測試聊天！
下載完成後，直接輸入對應的執行指令：
```
Bash
ollama run gemma:2b
# 或者
ollama run qwen:1.8b
```

Embedding 模型
```
ollama pull mxbai-embed-large
```

python 執行需要的套件
```
pip install langchain-ollama
pip install langchain-community
```

完整測試程式，包含Embeddings向量資料庫與llm聊天模型
{% highlight python linenos %}
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_community.embeddings import OllamaEmbeddings
from langchain_community.llms import Ollama

# 1. 初始化本地 Embedding
embeddings = OllamaEmbeddings(
    base_url="http://127.0.0.1:11434",
    model="mxbai-embed-large"
)
vector_store = InMemoryVectorStore(embedding=embeddings)
vector_store.add_texts(["減肥就是少吃多動", "鬼滅之刃真好看", "跑步是很好的運動", "節制食量，控制飲食", "早睡早起"])

input_text = "如何減肥"
retriever = vector_store.as_retriever(search_kwargs={"k": 2})

# 2. 初始化本地對話模型
model = Ollama(
    base_url="http://127.0.0.1:11434",
    model="qwen:1.8b",
    temperature=0
)

# 3. 專為傳統模型設計的純文字提示詞
# 3. 換成對小模型非常有效果的「命令式」提示詞
prompt = ChatPromptTemplate.from_template(
    "【重要任務】\n"
    "你必須完全根據下方提供的『參考資料』來回答使用者的問題。禁止說你找不到，請直接把參考資料的內容整理出來！\n\n"
    "參考資料：\n"
    "{context}\n\n"
    "使用者提出的問題：{input}\n\n"
    "請直接給出答案："
)

def format_func(docs):
    # 用逗號把找到的文本串起來，讓 AI 更好讀
    return "、".join([doc.page_content for doc in docs])

chain = (
    {"input": RunnablePassthrough(), "context": retriever | format_func}
    | prompt
    | model
    | StrOutputParser()
)

# 執行 RAG
print("本地 AI 正在思考中...")
res = chain.invoke(input_text)
print("\n本地 AI 回答的結果：")
print(res)
{% endhighlight %}