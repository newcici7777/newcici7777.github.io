---
title: 安裝VS在MAC(實作)
date: 2024-08-30
keywords: clang install
---



## 安裝`code`

打開`VS Code`，按住(Cmd+Shift+P)，並在以下視窗輸入

```
>shell command
```

![img]({{site.imgurl}}/clang/code1.png)

安裝成功會顯示以下畫面。

![img]({{site.imgurl}}/clang/code2.png)


## 安裝clang

打開`VS Code`，按住(⇧⌘X)，會出現延伸模組視窗。

並輸入c++

![img]({{site.imgurl}}/clang/clang1.png)

選擇C/C++，並安裝

![img]({{site.imgurl}}/clang/clang2.png)

## 修改 `~/.bash_profile`

打開終端機，輸入以下指令。

```
vi ~/.bash_profile
```

在最下方複製貼上以下內容。

```
# Add Visual Studio Code (code)
export PATH="\$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"
```

儲存離開。

在終端機再繼續輸入以下指令

```
source ~/.bash_profile
``` 

## 修改 `~/.zprofile`


在終端機再輸入以下指令。

```
cat << EOF >> ~/.zprofile
# Add Visual Studio Code (code)
export PATH="\$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"
EOF
```


## 檢查clang version

將終端機重開，輸入以下指令

```
clang --version
```

要確保出現如下圖示。

![img]({{site.imgurl}}/clang/clang3.png)

## `code .`

從終端機進入自建放程式碼的目錄，並在終端機輸入以下指令。

```
code .
```

![img]({{site.imgurl}}/clang/code3.png)

## 建立檔案

點擊新增檔案

![img]({{site.imgurl}}/clang/code4.png)

命名為helloword.cpp，並將以下程式碼貼上

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main()
{
    vector<string> msg {"Hello", "C++", "World", "from", "VS Code", "and the C++ extension!"};

    for (const string& word : msg)
    {
        cout << word << " ";
    }
    cout << endl;
}
{% endhighlight %}

## 執行

![img]({{site.imgurl}}/clang/code5.png)

點擊圖下紅框處

![img]({{site.imgurl}}/clang/code6.png)

## 修改tasks.json

確定command的路徑為`/usr/bin/clang++`，點擊helloworld.cpp再執行一次。

![img]({{site.imgurl}}/clang/code7.png)

## 執行結果

![img]({{site.imgurl}}/clang/code8.png)

