---
title: fs
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

1073