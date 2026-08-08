---
title: fs 檔案
date: 2026-08-04
keywords: node, fs
---
## fs
{% highlight javascript linenos %}
const fs = require('fs')
//console.log(__dirname)
// 使用__dirname 代表目前test1.js 所在的路徑
fs.readFile(__dirname + '/data/score.csv','utf8',function(err, dataStr) {
    if(err) {
        return console.log('fail' + err.message)
    }
    console.log('success' + dataStr)
    const arrOld = dataStr.split(',')
    const arrNew = []
    arrOld.forEach(item => {
        arrNew.push(item)
    })
    console.log(arrNew)
    // 每一個元素間有換行
    const newStr = arrNew.join('\n')
    console.log(newStr)
    fs.writeFile('./data/scoree_ok.csv', newStr, function(err){
        if(err) {
            return console.log('fail' + err.message)
        }
        console.log('sucess')
    })
})
{% endhighlight %}
打開VSCode終端機，執行以下指令
```
node 檔名.js
```
執行結果
```
success10,20,30,40
[ '10', '20', '30', '40' ]
10
20
30
40
sucess
```

## path
{% highlight javascript linenos %}
const path = require('path')
const pathStr = path.join('/a','/b/c','../','./d','e')
// 執行結果/a/b/d/e
// 沒有c，因為../會回上一層
console.log(pathStr)

const pathStr2 = path.join(__dirname, '/data/score.csv')
console.log(pathStr2)
{% endhighlight %}
```
/a/b/d/e
/Users/cici/Desktop/nodejs/node_proj/data/score.csv
```

{% highlight javascript linenos %}
const path = require('path')
const fs = require('fs')
//console.log(__dirname)
// 使用__dirname 代表目前test1.js 所在的路徑
// 使用 join，路徑./data多一個點不會失敗
fs.readFile(path.join(__dirname ,'./data/score.csv'),'utf8',function(err, dataStr) {
    if(err) {
        return console.log('fail' + err.message)
    }
    console.log('success' + dataStr)
})
{% endhighlight %}

取得檔名
{% highlight javascript linenos %}
const path = require('path')
const fpath = '/a/b/c/index.html'
// 取得檔名
var fileName = path.basename(fpath)
console.log(fileName)

// 取得副檔名
var extName = path.extname(fpath)
console.log(extName)
{% endhighlight %}
```
index.html
.html
```

## javascript exec()
```
/patten/.exec("要搜尋的字串")
```

以下範例，在exec("....")字串中，尋找e這個字母。
```
/e/.exec( "The best things in life are free!" )
```
尋找結果為
```
e
```

```
/<style>[\s\S]*<\/style>/
```
- `\s`（小寫 s）：代表空白字元（例如：空格、Tab 鍵、換行符號 \n 或 \r 等）。
- `\S`（大寫 S）：代表非空白字元（所有不是空白的字元，包括字母、數字、標點符號等）。
- [...]：代表字元集（Character Class），意思是「只要符合括號內任意一個字元即可」。
- [\s\S] ＝ 「是空白字元」或者「不是空白字元」。組合起來的意思就是：世界上所有的任何字元！

比對順序:
```
<style>：比對開頭的 <style> 標籤。

[\s\S]*：抓取中間任何字元（不管有沒有換行、有多長），直到...

<\/style>：比對結尾的 </style> 標籤（斜線前加 \ 是為了進行轉義）。
```

html
````
<html>
<head>
<style>
p.center {
  text-align: center;
  color: red;
}

p.large {
  font-size: 300%;
}
</style>
</head>
<body>

<h1 class="center">test1</h1>
<p class="center">test2</p>
<p class="center large">test3</p> 

</body>
</html>
```
第一階段 取出檔案
{% highlight javascript linenos %}
const fs = require('fs')
const path = require('path')
// 正規式
const regStyle = /<style>[\s\S]*<\/style>/
fs.readFile(path.join(__dirname, './data/index.html'), 'utf-8',function(err,dataStr){
    if(err) return console.log('load file fail'+ err.message)
    // 輸出檔案內容是html的內容
    console.log(dataStr)
})
{% endhighlight %}

第二階段，取出css
{% highlight javascript linenos %}
const fs = require('fs')
const path = require('path')
// 正規式
const regStyle = /<style>[\s\S]*<\/style>/
fs.readFile(path.join(__dirname, './data/index.html'), 'utf-8',function(err,dataStr){
    if(err) return console.log('load file fail'+ err.message)
    const r1 = regStyle.exec(htmlStr)
    // 正規式匹配的結果會在陣列r1[索引0]
    console.log(r1[0])
})
{% endhighlight %}
```
<style>
p.center {
  text-align: center;
  color: red;
}

p.large {
  font-size: 300%;
}
</style>
```

第三階段，把`<style>`與`</style>`替代成空白
{% highlight javascript linenos %}
const fs = require('fs')
const path = require('path')
// 正規式
const regStyle = /<style>[\s\S]*<\/style>/
fs.readFile(path.join(__dirname, './data/index.html'), 'utf-8',function(err,dataStr){
    if(err) return console.log('load file fail'+ err.message)
    const r1 = regStyle.exec(htmlStr)
    console.log(r1[0])
    const newCss = r1[0].replace('<style>','').replace('</style>','')
})
{% endhighlight %}
```

p.center {
  text-align: center;
  color: red;
}

p.large {
  font-size: 300%;
}
```

第四階段，把以上內容，寫入到index.css檔案中
{% highlight javascript linenos %}
const fs = require('fs')
const path = require('path')
// 正規式，\
const regStyle = /<style>[\s\S]*<\/style>/
function resolveCSS(htmlStr) {
    const r1 = regStyle.exec(htmlStr)
    const newCss = r1[0].replace('<style>','').replace('</style>','')
    fs.writeFile(path.join(__dirname, './data/index.css'),newCss,err=>{
        if(err) return console.log('fail'+err.message)
            console.log('success')
    })
}

fs.readFile(path.join(__dirname, './data/index.html'), 'utf-8',function(err,dataStr){
    if(err) return console.log('load file fail'+ err.message)
    resolveCSS(dataStr)
})
{% endhighlight %}

第五階段，修改原本的html，改成引入index.css
{% highlight javascript linenos %}
const fs = require('fs')
const path = require('path')
// 正規式，\
const regStyle = /<style>[\s\S]*<\/style>/
function resolveCSS(htmlStr) {
    const r1 = regStyle.exec(htmlStr)
    //console.log(r1[0])
    const newCss = r1[0].replace('<style>','').replace('</style>','')

    fs.writeFile(path.join(__dirname, './data/index.css'),newCss,err=>{
        if(err) return console.log('fail'+err.message)
            console.log('success')
    })
}
function resolveHTML(htmlstr) {
    // 把原本css的位置，換成以下內容
    const newHTML = htmlstr.replace(regStyle, '<link rel="stylesheet" href="./index.css" />')
    // 新的html寫入到index.html
    fs.writeFile(path.join(__dirname,'./data/index.html'),newHTML,function(err){
        if(err) return console.log("fail" + err.message)
        console.log('sucess')
    })
}

fs.readFile(path.join(__dirname, './data/index.html'), 'utf-8',function(err,dataStr){
    if(err) return console.log('load file fail'+ err.message)
    resolveCSS(dataStr)
    resolveHTML(dataStr)
})
{% endhighlight %}
修改原本的index.html，把原本css的位置，改成`<link rel="stylesheet" href="./index.cs" />`
```
<html>
<head>
<link rel="stylesheet" href="./index.cs" />
</head>
<body>

<h1 class="center">test1</h1>
<p class="center">test2</p>
<p class="center large">test3</p> 

</body>
</html>
```