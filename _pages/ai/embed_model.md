---
title: Embeddings model
date: 2026-05-20
keywords: Langchain, Gemini, Embeddings model
---
參考文件
<https://docs.langchain.com/oss/python/integrations/embeddings/google_generative_ai>

{% highlight python linenos %}
from langchain_google_genai import GoogleGenerativeAIEmbeddings

embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")
vector = embeddings.embed_query("hello, world!")
print(vector[:5])
{% endhighlight %}
```
[-0.0102722915, -0.0023818077, 0.0279904, -0.0067098644, -0.013417502]
```

多筆
{% highlight python linenos %}
from langchain_google_genai import GoogleGenerativeAIEmbeddings

embeddings = GoogleGenerativeAIEmbeddings(model="gemini-embedding-2-preview")
vectors = embeddings.embed_documents(
    [
        "Today is Monday",
        "Today is Tuesday",
        "Today is April Fools day",
    ]
)
print(len(vectors))
print(vectors[0][:5])
{% endhighlight %}
```
1
[-0.0043718526, 0.018828297, 0.0039531393, -0.0058853854, 0.015948385]
```