---
title: Chain
date: 2026-05-20
keywords: Langchain, Chain
---
{% highlight python linenos %}
class Test(object):
    def __init__(self, name):
        self.name = name
    def __or__(self, other):
        return MySequence(self, other)
    def __str__(self):
        return self.name

class MySequence(object):
    def __init__(self, *args):
        self.sequence = []
        for arg in args:
            self.sequence.append(arg)
    def __or__(self, other):
        self.sequence.append(other)
        return self
    def run(self):
        for i in self.sequence:
            print(i)

if __name__ == '__main__':
    a = Test('a')
    b = Test('b')
    c = Test('c')

    d = a | b | c
    d.run()
{% endhighlight %}
```
a
b
c
```

## 繼承關係
```
Runnable 祖先類別
      ↑
RunnableSerializable 祖父類別
      ↑
BasePromptTemplate 父類別
      ↑
PromptTemplate 子類別
```

Runnable 祖先類別中，有一個`__or__()` 方法，會傳回RunnableSequence 物件。<br>

RunnableSequence 物件也有`__or__()` 方法。<br>

代入上面的or範例，Runnable 類別等同於Test 類別。<br>
RunnableSequence 類別，等同 MySequence 類別。<br>

## chain的類別為 RunnableSequence
RunnableSequence 裡面有 `__or__` 函式，代表可以使用 `|` 也就是 `__or__` 函式，使用 `a | b | c | d`的方式，把prompt 作為參數，傳給 llm。 
{% highlight python linenos %}
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

prompt = PromptTemplate.from_template("你是一個AI助手")
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
chain = prompt | llm
print(type(chain))
{% endhighlight %}
```
<class 'langchain_core.runnables.base.RunnableSequence'>
```
