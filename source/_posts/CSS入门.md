---
title: CSS入门
date: 2019-02-20 23:11:23
tags:
---
## CSS 
#### 全称：Cascading Style Sheets 层叠样式表  
1. 有不懂的看CSS文档（全英） 或者 问  
2. css 2.1 是最广泛的，css 3 已经八年了  
3. 周边工具：  
&nbsp; - LESS CSS：一种简化、功能变多的CSS语言  
&nbsp; - SASS：一种简化、功能变多(比上面更多)的CSS语言  
&nbsp; - postCSS:一种CSS处理程序，能帮我们纠正我们写错的地方  

先纯手写，一个一个敲，打基础一个月。再去用这些周边工具。

---

## CSS学习资源：  
1. 想学什么就google搜：xxx mdn  
2. CSS Tricks：上面有很多代码片段，点击网页导航栏中的 Snippets（片段） 也可以直接goole搜索：比如居中: center css tricks(注意用英文搜索)  
3. 看阮一峰 CSS 的博客 和 张鑫旭 CSS 的博客  
4. Codrops: 上面有很多炫酷的CSS效果  

建议：  
- 中文学习资源只看 3   
- 英文资源看 1,2,4   
- 如果想快速上手就先写个小demo再学理论，或者仔细看[css规范文档](http://www.ayqy.net/doc/css2-1/cover.html)，，3*8小时看到第18章即可直接成为CSS大神！ 
---
## 引入CSS的三/四种方式：  
1. 内联style属性：通过在标签内添加style属性实现样式改变 
2. style标签：也可以不写在标签里，单独的在head标签内写style标签，好处：让标签变得简单   
3. 外部文件 css link：把样式都放在一个css文件中，在html文件head标签中使用link标签链接css文件。  
如 <link rel="stylesheet" href="./a.css"> 这里的rel是 relationship 关系  
4. @import url(./b.css);（绝对路径相对路径都行）在一个a.CSS文件中还要引入另一个b.CSS文件，就在a.css内第一行写这行代码   

（3最常用，4不常用）
