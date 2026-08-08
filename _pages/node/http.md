---
title: http
date: 2026-08-07
keywords: node, http
---
{% highlight java linenos %}
const http = require('http')
const server = http.createServer()
server.on('request', (req, res) =>{
    console.log("someone visit our web server.")
    // 注意！格式化字串是用`內容`，不是雙/單引號
    console.log(`request url is ${req.url}, metoh ${req.method}`)
})
server.listen(8080, ()=>{
    console.log("server runing http://127.0.0.1:8080")
})
{% endhighlight %}
執行
```
node 檔名.js
```

執行結果
```
% node test3.js
server runing http://127.0.0.1:8080
```

開啟網頁，執行http://127.0.0.1:8080
```
someone visit our web server.
request url is /, metoh GET
```

執行postman
![img]({{site.imgurl}}/node/postman.png)<br>

網址輸入:http://127.0.0.1:8080/test.html
```
someone visit our web server.
request url is /test.html, metoh GET
```

將回應寫在客戶端的瀏覽器上
{% highlight javascript linenos %}
const http = require('http')
const server = http.createServer()
server.on('request', (req, res) =>{
    var str = `網址 ${req.url}, metoh ${req.method}`
    // 處理中文
    res.setHeader('Content-Type','text/html;charset=utf-8')
    // 寫到客戶端的瀏覽器上
    res.end(str)
})
server.listen(8080, ()=>{
    console.log("server runing http://127.0.0.1:8080")
})
{% endhighlight %}

執行網頁
![img]({{site.imgurl}}/node/web.png)<br>

載入網頁，網址輸入
{% highlight javascript linenos %}
const http = require('http')
const fs = require('fs')
const path = require('path')
const server = http.createServer()
server.on('request', function(req,res){
    const url = req.url
    const fpath = path.join(__dirname,url)
    fs.readFile(fpath, 'utf8', (err,dataStr) => {
        if(err) return res.end('404 not found')
        res.end(dataStr)
    })
})

server.listen(8080,function(){
    console.log('server at http://127.0.0.1:8080 ')
})
{% endhighlight %}