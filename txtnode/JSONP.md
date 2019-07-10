预习：[阮一峰 AJAX](http://javascript.ruanyifeng.com/bom/ajax.html)

---

## 如何发请求？
用 form 可以发请求，但是会刷新页面或新开页面  
用 a 可以发 get 请求，但是也会刷新页面或新开页面  
用 img 可以发 get 请求，但是只能以图片的形式展示  
用 link 可以发 get 请求，但是只能以 CSS、favicon 的形式展示  
用 script 可以发 get 请求，但是只能以脚本的形式运行  

有没有什么方式可以实现

1. get、post、put、delete 请求都行
2. 想以什么形式展示就以什么形式展示

## AJAX
Jesse James Garrett 讲如下技术取名叫做 AJAX：异步的 JavaScript 和 XML

1. 使用 XMLHttpRequest 发请求  
2. 服务器返回 XML 格式的字符串  
3. JS 解析 XML，并更新局部页面  

### 面试题：请使用原生JS发送AJAX请求

## 如何使用 XMLHttpRequest
1.request.onreadystatechange === 4 表示请求完成  
## JS VS JSON
```
js               json
null             null
['a','b']        ["a","b"]
{name: 'om'}     {"name":"om"}
'om'             "om"
undefined        没有
function fn(){}  没有
var a={}
a.self = a       做不到
复杂对象         只有hash，没有原型链
```
1. JSON没有抄袭 undefined 和 function
2. JSON的字符串首尾必须是双引号""