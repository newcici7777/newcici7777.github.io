---
title: md5
date: 2026-06-08
keywords: python, md5
---
以下程式碼，產生的md5，只有最後一個不一樣。
{% highlight python linenos %}
import hashlib
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

if __name__ == '__main__':
    r1 = get_string_md5("小美")
    r2 = get_string_md5("小美")
    r3 = get_string_md5("小美2")
    print(r1)
    print(r2)
    print(r3)
{% endhighlight %}
```
cb92be636acc59e649b89668faf7008b
cb92be636acc59e649b89668faf7008b
6f7e559e7bbfc1998a5c994ca95a86df
```

config.py
```
md5_path = "./md5.text"
```

{% highlight python linenos %}
import os
import config_data as config
import hashlib

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
        self.chroma = None
        self.spliter = None

if __name__ == '__main__':
	# 建立檔案
    check_md5('cb92be636acc59e649b89668faf7008b')
    # 寫入檔案
    save_md5('cb92be636acc59e649b89668faf7008b')
	# 讀取檔案
    print(check_md5('cb92be636acc59e649b89668faf7008b'))
{% endhighlight %}
```
True
```