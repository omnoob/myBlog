个人笔记，暂时没有附带源代码，见谅。  
第一部分阐述什么是JSONP，附带面试题一个。  
第二部分是发展历程，img法，script法，JSONP法。没有源代码阅读会笔记困难，再次见谅。  
第三部分随笔记录一些基础知识。

---
# JSONP

请求方：frank.com 的前端程序员（浏览器）  
响应方：jack.com 的后端程序员（服务器）  

1. 请求方创建 script，src 指向响应方，同时传一个查询参数  ?callbackName=yyy  
2. 响应方根据查询参数callbackName，构造形如  
i.yyy.call(undefined, '你要的数据')  
ii.yyy('你要的数据')  
&nbsp;&nbsp;&nbsp;这样的响应  
3. 浏览器接收到响应，就会执行 yyy.call(undefined, '你要的数据')  
4. 那么请求方就知道了他要的数据  

这就是 JSONP  
约定：
1. callbackName -> callback
2. yyy -> 随机数 比如xxx123145123()

## 面试题：  
请问JSONP为什么不能发POST请求？  
第一句话： 因为JSONP是通过动态创建script实现的。  
第二句话： 动态创建script的时候只能用GET，不能用POST

---

- **img更新法**：用img悄无声息地创建一个请求。只能get，不能post。
如果状态码是2XX就是请求成功，3xx是重定向，4xx就是客户端错误，5xx是服务器错误。
```
button.addEventLister('click', (e)=>{
    let image = document.creatElement('img')
    img.src = '/pay'
    image.onload = function(){ // 状态码是 200~299 则表示成功
        alert('打钱成功')
        amount.innerText = amount.innerText - 1
    }
    image.onerror = function(){ // 状态码大于等于 400 则表示失败
        alert('打钱失败')
    }
})
```
- **script请求法（SRJ）**：比img请求法更快；浏览器会执行script的内容即浏览器返回的内容，所以就不需要onload、onerror了；  
**SRJ**： server rendered JavaScript 服务器返回的js，这是AJAX出来前的无刷新局部更新页面内容的方案。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;特点：script可以使用另一个网站的JS。
```
<script>
button.addEventLister('click', (e)=>{
    let script = document.creatElement('script')
    script.src = '/apy'
    // 只这样做是不行的。必须把下句代码放在body里才可以，不能放在js文件
    // 只有把下句代码放在页面里，浏览器才会发请求
    document.body.appenChild(script)
    script.onerror = function(){
        alert('打钱失败')
    }
})
</script>

index.js:
response.write(`amount.innerText = amount.innerText - 1`)
```
但是每次打钱成功都会创建一个script，html就变的很丑，所以我们要打钱成功或失败后删除这个动态创建的script：
```
//只需监听script的onload和onerror，都删除这个script即可
script.onload = function(){
    e.currentTarget.remove()
    }
script.onerror = function(){
    alert('打钱失败')
    e.currentTarget.remove()
}
```
script请求法过程描述：
```
若用户点击了打钱的按钮，就创建一个script，script的src就是要请求的路径，然后把script放到页面里，这样浏
览器就会去发起这个路径的get请求；如果get成功了，它首先会执行服务器返回的js的响应，这个响应就是操作页面局
部的刷新，为什么会把响应当成script执行呢?一是因为我们设置的response.setHeader就是application/java
script；二是我前端确实把它放在script标签里的。执行之后，用户就看到金额减少了，减少之后我们就去监听，
无论成功失败，都会删除这个script。
```
# 但是
如果A网站的程序员要去调用B网站的JS，就必须对B网站的页面细节知道的很清楚，否则接口就对不上，这样耦合太大了！所以我们要解除耦合！  

方法：我调用个函数就行了，我不关心里面是什么
```
response.write(`amount.innerText = amount.innerText - 1`) 
改成
response.write(`xxx.call(undefined,'打钱成功'`) 
同时在A网站JS里，监听xxx函数。
```
这个函数名xxx双方是怎么传递的呢？
```
A:script.src='http://B.com/pay?callback=xxx'
B:response.write(`$(query.callback).call(undefined,'打钱成功'`) 
```
代码细节1:
![](https://user-gold-cdn.xitu.io/2019/7/10/16bdb827adcd75c1?w=659&h=307&f=png&s=128247)


# JSONP

请求方：frank.com 的前端程序员（浏览器）  
响应方：jack.com 的后端程序员（服务器）  

1. 请求方创建 script，src 指向响应方，同时传一个查询参数  ?callbackName=yyy  
2. 响应方根据查询参数callbackName，构造形如  
i.yyy.call(undefined, '你要的数据')  
ii.yyy('你要的数据')  
&nbsp;&nbsp;&nbsp;这样的响应  
3. 浏览器接收到响应，就会执行 yyy.call(undefined, '你要的数据')  
4. 那么请求方就知道了他要的数据  

这就是 JSONP  
约定：
1. callbackName -> callback
2. yyy -> 随机数 比如xxx123145123()
```
functionName = 'om'+parseInt(Math.random()*100000,10)
window[functionName] = function(result){...}
script.src = 'http://jack.com:8002/pay?callback=' + functionName
```

JSONP
```
button.addEventListener('click', (e)=>{
    let script = document.createElement('script')
    let functionName = 'frank'+ parseInt(Math.random()*10000000 ,10)
    window[functionName] = function(){  // 每次请求之前搞出一个随机的函数
        amount.innerText = amount.innerText - 0 - 1
    }
    script.src = '/pay?callback=' + functionName
    document.body.appendChild(script)
    script.onload = function(e){ // 状态码是 200~299 则表示成功
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
    script.onerror = function(e){ // 状态码大于等于 400 则表示失败
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
})
```

```
//后端代码
...
if (path === '/pay'){
    let amount = fs.readFileSync('./db', 'utf8')
    amount -= 1
    fs.writeFileSync('./db', amount)
    let callbackName = query.callback
    response.setHeader('Content-Type', 'application/javascript')
    response.write(`
        ${callbackName}.call(undefined, 'success')
    `)
    response.end()
}
```
# 写了这么多，jQuery已经帮咱写好了！！！！！
[代码原网站](https://learn.jquery.com/ajax/working-with-jsonp/)
```
button.addEventListener('click', (e)=>{
     $.ajax({ // 这个名字跟ajax没有半毛钱关系，ajax是ajax，jsonp是jsonp
     url: "http://jack.com:8002/pay",
     dataType: "jsonp",
     success: function( response ) {
         if(response === 'success'){
         amount.innerText = amount.innerText - 1
         }
     }
     })
    
     $.jsonp()
})
```
## 面试题：  
请问JSONP为什么不能发POST请求？  
第一句话： 因为JSONP是通过动态创建script实现的。  
第二句话： 动态创建script的时候只能用GET，不能用POST

-------------------------------------
- [onload](http://www.w3school.com.cn/jsref/event_onload.asp)：onload 事件会在页面或图片加载完成后立即发生。
- [onerror](http://www.w3school.com.cn/jsref/event_onerror.asp)：onerror 事件会在文档或图像加载过程中发生错误时被触发。
在装载文档或图像的过程中如果发生了错误，就会调用该事件句柄。
- [alert()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/alert)：显示一个警告对话框,上面显示有指定的文本内容以及一个"确定"按钮。
- [Location.reload()](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/reload)：用来刷新当前页面。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该方法只有一个参数，当值为 true时，将强制浏览器从服务器加载页面资源，当值为 false 或者未传参时，浏览器则可能从缓存中读取页面。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该方法在跨域调用（执行该方法的脚本文件的域和 Location 对象所在页面的跨不同）时，将会抛出 DOMException 异常。  
- [ASP Response 对象](http://www.w3school.com.cn/asp/asp_ref_response.asp)：于从服务器向用户发送输出的结果。ASP其余对象参考[链接](http://www.w3school.com.cn/asp/asp_intro.asp)
response.write()：向输出写指定的字符串。  
response.end()：停止处理脚本，并返回当前的结果。  
response.setHeader(name, value)：例如`response.setHeader('Content-Type', 'text/html');`  
response.statusCode = 2xx/3xx/4xx/5xx ：此属性控制在刷新响应头时将发送到客户端的状态码。
- [node.js](http://nodejs.cn/api/http.html#http_response_setheader_name_value)：Node.js就是这样一个服务器端的、非阻断式I/O的、事件驱动的JavaScript运行环境。（去茶叶铺卖茶叶例子）  
[Node.js的应用是通过javascript开发的，然后直接在Google的变态V8引擎上跑。用了Node.js，你就不用担心用户端的请求会在服务器里跑了一段能够造成阻塞的代码了。因为javascript本身就是事件驱动的脚本语言。](https://www.zhihu.com/question/33578075)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;服务器端JavaScript处理：server-side JavaScript execution  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;非阻断/异步I/O：non-blocking or asynchronous I/O  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;事件驱动：Event-driven  
- 占位符：`&&&amount&&&` 这是一个特殊的占位符，整体都是，为了表示这是我的占位符，别人别碰。
- fs: `fs`是`filesystem`的缩写，该模块提供本地文件的读写能力，基本上是POSIX文件操作命令的简单包装。但是，这个模块几乎对所有操作提供异步和同步两种操作方式，供开发者选择。    [FS讲义](http://javascript.ruanyifeng.com/nodejs/fs.html#toc2) [FS文档](http://nodejs.cn/api/fs.html#fs_fs_writefilesync_file_data_options)
```
异步 fs.readFile(文件的路径, 读取完成后的回调函数)
同步 fs.readFileSync(文件路径,'utf8')

     fs.readFileSync()的第二个参数可以是一个表示配置的对象，也可以是一个表示文本文件编码的字符串。
     默认的配置对象是{ encoding: null, flag: 'r' }，即文件编码默认为null，读取模式默认为r（只读）。
     如果第二个参数不指定编码（encoding），readFileSync方法返回一个Buffer实例，否则返回的是一个字符串。
     
     Buffer对象是Node处理二进制数据的一个接口,它是Node原生提供的全局对象，可以直接使用。
     buffer文档：https://javascript.ruanyifeng.com/nodejs/buffer.html

```
- [form](http://www.w3school.com.cn/tags/tag_form.asp)(html标签)： 一旦提交，就刷新当前页面
```
action="URL" 规定当提交表单时向何处发送表单数据。
method="get/post" 规定用于发送 form-data 的 HTTP 方法。 
target="" 规定在何处打开 action URL。
```
- [input](http://www.w3school.com.cn/tags/tag_input.asp): 用于搜集用户信息。根据不同的 type 属性值，输入字段拥有很多种形式。输入字段可以是文本字段、复选框、掩码后的文本控件、单选按钮、按钮等等。 `value="xxx" 规定input元素的值是xxx`  