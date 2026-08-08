---
title: module exports 共享變數
date: 2026-08-08
keywords: node, module exports
---
一個js檔案就是一個module，module之間無法共享變數，需要透過module exports來共享變數。

每一個檔案都有自己的module變數。
{% highlight javascript linenos %}
console.log(module)
{% endhighlight %}
其中的exports屬性，就是本身這個檔案，檔案本身就是module，分享了什麼變數給別人使用。
```
  path: '/Users/cici/Desktop/nodejs/node_proj',
  exports: {},
  filename: '/Users/cici/Desktop/nodejs/node_proj/test6.js',
```

雖然說屬性名為exports，但實際上，它指向的是module.exports，並非exports，使用時以module.exports為主。

test5.js
{% highlight javascript linenos %}
const username = 'Mary'
module.exports.username = username
exports = {
    gender: "男"
}
function sayHello() {
    console.log('Hello, I am '+ username)
}
{% endhighlight %}

test6.js
{% highlight javascript linenos %}
// 副檔名可不加.js
const custom = require('./test5')
console.log(custom)
{% endhighlight %}
由執行結果來看，指向的永遠為module.exports，並非exports。
```
$node test6.js
{ username: 'Mary' }
```

------------
合併module.exports與exports

test5.js
{% highlight javascript linenos %}
const username = 'Mary'
const age = 20
exports = {
    gender: "男"
}
module.exports = exports
module.exports.username = "Tom"
function sayHello() {
    console.log('Hello, I am '+ username)
}
{% endhighlight %}

test6.js
{% highlight javascript linenos %}
const custom = require('./test5')
console.log(custom)
{% endhighlight %}
執行
```
$node test6.js
{ gender: '男', username: 'Tom' }
```
1305