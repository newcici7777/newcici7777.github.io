---
title: json
date: 2026-05-15
keywords: python, json
---
## json 介紹
```
{
  "key": value,
  "key": value
}
```
key為字串，用「雙引號」包起來。<br>
value可以為:<br>
- 數字
- 字串
- list
- json 陣列
- json 物件

```
json = {
	"name": "Cici", 
	"age": 20, 
	"hobby": ["Dance", "Sign", "Reading"],
	"address": {"國家": "台灣", "郵遞區號": 001, "address": "xxxxx"}
}
```

Json Array
```
[{}, {}, {}]
[
  {"name": "Mary", "age": 10},
  {"name": "Bill", "age": 20},
]
```

### json 與 dict差別
dict的key是用單引號，json的key是雙引號。<br>
json的字串都是用雙引號，單引號在python中是字串，例其它語言如java, c++ 是字元。<br>
```
dict = {'name': 'Cici', 'age': 20, 'gender': '女'}
json = {"name": "Cici", "age": 20, "gender": "女"}
```

## json.dumps() 與 json.loads() 語法
### json.dumps() 語法
json.dumps()將dict或list轉成Json字串，
```
result = json.dumps(dict 或 list, ensure_ascii = False)
```
- 參數1，dict或list。
- 參數2，ensure_ascii = False 確保中文能正常顯示。
- 傳回值為Json字串。

### json.loads() 語法
json.loads()將Json轉成dict或list。
```
result = json.loads(Json字串)
```
- 參數1，Json字串。
- 傳回值為dict或list。

## json.dumps()

{% highlight python linenos %}
import json
d = {
    "name": "Cici",
    "age": 20,
    "gender": "女"
}

s = json.dumps(d, ensure_ascii=False)
print(s)
{% endhighlight %}
```
{"name": "Cici", "age": 20, "gender": "女"}
```

## json.loads()
使用單引號，把json字串包起來。<br>
{% highlight python linenos %}
import json
json_str = '{"name": "Cici", "age": 20, "gender": "女"}'
res_dict = json.loads(json_str)
print(res_dict)
{% endhighlight %}
```
{'name': 'Cici', 'age': 20, 'gender': '女'}
```

## json 檔案讀與寫
- dump 把dict 轉成 json格式 存入檔案
- load 把 json格式 轉成 dict

把dict obj 轉成 json格式 存入檔案，第二個參數有f
```
obj = {
    "name": "王大明",
    "age": 18,
    "gender": "male",
}
json.dump(obj, f)
```

把檔案裡的json格式轉成dict，以下參數有f
```
obj = json.load(f)
```

### json 寫入檔案
indent 為縮排空格
{% highlight python linenos %}
import json

obj = {
    "name": "王大明",
    "age": 18,
    "gender": "male",
}
with open("data/session.json", "w", encoding="utf-8") as f:
    json.dump(obj, f, ensure_ascii=False, indent=2)
{% endhighlight %}
```
{
  "name": "王大明",
  "age": 18,
  "gender": "male"
}
```

### 讀取json檔案
{% highlight python linenos %}
import json

with open("data/session.json", "r", encoding="utf-8") as f:
    obj = json.load(f)
    print(obj)
{% endhighlight %}
```
{'name': '王大明', 'age': 18, 'gender': 'male'}
```

### 物件格式是dict
{% highlight python linenos %}
import json
with open("data/session.json", "r", encoding="utf-8") as f:
    obj = json.load(f)
    print(type(obj))
{% endhighlight %}
```
<class 'dict'>
```