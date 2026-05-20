---
title: FewShotPromptTemplate
date: 2026-05-20
keywords: Langchain, Gemini, FewShotPromptTemplate
---
語法
```
few_shot_template.invoke().to_string()
```

{% highlight python linenos %}
from langchain_core.prompts import FewShotPromptTemplate,PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

example_prompt = PromptTemplate.from_template(
    "單字:{word}, 相反詞:{anti-word}")
example_data = [
    {"word":"大", "anti-word":"小"},
    {"word":"上", "anti-word":"下"},
]
few_shot_template = FewShotPromptTemplate(
    example_prompt = example_prompt,
    examples = example_data,
    prefix = "請給出單字的相反詞，例如:",
    suffix = "根據範例告訴我，{input_word} 的相反詞是？",
    input_variables= ["input_word"],
)
prompt_text = few_shot_template.invoke(input={"input_word":"長"}).to_string()
llm = ChatGoogleGenerativeAI(model="gemini-3-flash-preview", temperature=0)
res = llm.invoke(input=prompt_text)
print(res.text)
{% endhighlight %}
```
單字:長, 相反詞:**短**
```