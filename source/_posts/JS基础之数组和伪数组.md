## 一、Array 
### 什么是数组？  
1.从人类的角度看，数组是按次序排列的一组值。  
2.从JS的角度看，是用Array定义的一个对象。  
`Array.prototype{.push(),.pop(),.shift(),.join(),...}`  

### 1.基础用法  
用法1：`var a = Array[3]`,得到的是长度为3的数组，且值为undefined。  
用法2：`var a = Array[3,3]`,得到的是长度为2，值为{3，3}的数组。  

    ```
    Array(2)  
    0:3  
    1:3  
    length：2
    __proto__:Array(0)
    ```
    
![](https://user-gold-cdn.xitu.io/2019/7/1/16bae066f22658a9?w=978&h=673&f=png&s=180668)  
 
小总结：
![](https://user-gold-cdn.xitu.io/2019/7/1/16bae1570ace8600?w=1165&h=682&f=png&s=192997)  
## 2.数组和对象的区别  
对比：a=[1,2,3] 和 object={0:1 1:2 2:3 length=3 }    
本质区别：他们的__proto__指向不同即原型链不同，如图所示：
![](https://user-gold-cdn.xitu.io/2019/7/3/16bb686a38195ce7?w=1142&h=724&f=png&s=306595"数组和对象的区别是原型链不同")  
实际上，数组是不是数组，取决于使用者要不要把它当成数组：  
```
var obj = {
    0:1, 1:2, 2:3 ,3:4, length:4
}
当成数组：
for(let i=0; i<obj.length;i++){
    console.log(obj[i])
}
当成对象：
for(let i in obj){
    console.log(obj[i])
}
```  
## 3.伪数组  
定义:原型链(\__proto__)没有Array.prototype，那这个对象就是伪数组。  
用途：目前JS只有一个伪数组`arguments`（arguments就是接收到的参数）  
```
function (){
    console.dir(arguments)
}
f(1,2,3)
>> Argument(3)
    0:1
    1:2
    2:3
    callee:f f()
    length:3
    Symbol(symbol.iterator):f value()
    __proto__:Object
```
## 4.Array.API   
1. **a.forEach**：用于调用数组的每个元素，并将元素传递给回调函数。
```
function forEach(array,fn){ // 遍历这个数组，并将值传递给fn，故fn必须接收两个参数（具体看文档）
    for(let i=0; i<array.length;i++){
        fn(array[i],i) //fn(value,key)
    }
}
var a=['a','b','c']
a.forEach(function(value,key){ //此处value、key就是从a传递过来的参数
    console.log(value,key)
})
结果：
      a 0
      b 1
      c 2
      undefined
注意：在JS中，a.forEach(fn)自动把数组a的每个元素传递给fn。（利用this）
```
一句话结论：**a.forEach接收一个函数，这个函数必须接收三个参数，依次是a的value、key和a本身,且返回值是undefined**。
```
a=['fff','jjj','kkk']
a.forEach(function(b,c,d){
    console.log(b,c,d)    
})
结果：
    fff 0 (3) ['fff','jjj','kkk']
    jjj 1 (3) ['fff','jjj','kkk']
    kkk 2 (3) ['fff','jjj','kkk']
```
2. **a.sort**：数组排序（一般是快排）（会改变原数组）
```
原理：a.sort接收一个函数，这个函数必须接收两个参数x、y,返回值有三种：
①a.sort(function(x,y){return x-y})              此为从小到大排列
    返回值>0, 表示前面数字大，要放后面。
    返回值=0，表示两个数字位置相等。
    返回值<0. 表示前面数字小，要放前面
    
②a.sort(function(x,y){return y-x})              此为从大到小排列
    同理，此处x-y变成y-x
    
实际应用中，可两者都试一试，哪个结果是我们想要的就用哪个。  

例子：
    a = ['马云'，'马化腾','李彦宏']，按照财富值从大到小排列
    hash = {
        '马云':167,
        '马化腾':376,
        '李彦宏':228
    }
    a.sort(function(x,y){return hash[x] - hash[y]}) 
    结果：['马云'，'李彦宏','马化腾'] // 这个结果是错的，所以x-y不行，改成y-x
    a.sort(function(x,y){return hash[y] - hash[x]}) 
    结果：'马化腾','李彦宏'，['马云']
```
实际使用sort排序时，要给一个谁大谁小的凭证，如例子中的hash，虽然a中是人名无法直接比较，但是我们凭借hash排出了顺序，x-y不行就用y-x就好了。  
3. **a.join**：在a的每个元素之间插入参数
```
a=[1,2,3]
a.join(插入)
结果："1插入2插入3"
常用于数组变字符串：a.join(,) 等价于 a + ''
```
4. **a.concat**：合并多个数组；合并多个字符串；连接多个数组
```
普通用法：
        var a = [1,2,3]
        var b = [4,5,6]
        a.concat(b)
        结果：[1,2,3,4,5,6]
特殊用法：复制一个数组，且复制的数组与原数组不相等
        var a = [1,2,3]
        var b = a.concat([])
        a === b // false
        var c = a 
        c == a // true
```
5. **a.map**：功能与a.forEach完全相同，但返回值不是undefined，而是将函数返回的每一个值收集起来组成一个新的数组，新数组的长度与原数组相同。(map为映射的意思)
```
a = [1,2,3]
a.forEach(function(){}) // >undefined
a.map(function(value,key){
    return value * 2
})                     // > [2,4,6]
转换为箭头函数：a.map(value => value*2)
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return的格式无限制，可根据实际需求写返回值的格式。  
6. **a.filter**：filter是过滤的意思，根据return的条件筛选出元素并组成一个新的数组。
```
a = [1,2,3,4,5,6,7,8,9,10]
a.filter(function(value,key){
    return value % 2 === 0 //除以2的余数为0，即偶数
})
结果：
    [2,4,6,8,10]
    
filter与map连用：
a = [1,2,3,4,5,6,7,8,9,10]
a.filter(function(value,key){
    return value % 2 === 0         
}).map(function(value,key){    //[2,4,6,8,10].map...
    return value * value
})
结果：
    [4,16,36,64,100]
```
7. **a.reduce**：遍历数组a，每一次都取一个结果，并把这个结果放在下一项上。
```
a = [1,2,3]
a.reduce(function (sum,n){  // 求数组a的元素总和
    return sum + n 
},0)
此处function也必须接收两个值，一个是sum，一个是n；除function外还要接收一个初始值sum=0
转换为箭头函数：a.reduce((sum,n) => sum + n,0)
```
### 总结：
1. *map 可以用 reduce 表示*
```
a = [1,2,3]                          //每项元素乘2
a.reduce(function(arr,n){ //arr是上一次的数组，n是数组a的每个元素
    arr.push(n*2)
    return arr
},[])
结果：
    [2,4,6]
```
2. *filter 可以用 reduce 表示*
```
a = [1,2,3,4,5,6,7,8,9,10]           //找出偶数
a.reduce(function(arr,n){ //arr是上一次的数组，n是数组a的每个元素
    if(n % 2 === 0){
        arr.push(n)
    }
    return arr
},[])
结果：
    [2,4,6,8,10]
```

## 二、Function  
Function.prototype{.call(),.bind(),.apply()}  
用法：`new Function ([[参数1,参数2,...,参数n],]函数体)`      
&nbsp;&nbsp;&nbsp;注：[..]内的内容为可选内容，可有可无。  
例如：`var f = new Function('a','b','return a+b')`  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`f(1,2)`=>`3`  
**f**unction 是一个**关键字**，用来声明一个函数：`function f(){}`  
**F**unction 是一个**全局变量**，用法为：`var f = new Function('x','y','x+y')`  
注意基础概念来进行区分。

* 声明function的几种方法：  
```
1.具名函数：直接一步到位  
function f(){
    return
}
2.匿名函数:先声明一个f，然后f指向匿名函数
var f
f = function (){
    teturn
}
3.Function：此法一般不用
new Function('x','y','s+y')
``` 
