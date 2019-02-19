---
title: Git新手教程
date: 2019-02-05 01:35:30
tags:
---
---
title: Git新手教程
date: 2019-02-05 01:08:16
tags:
---
本文环境为windows10 工具为git bash 。

---
# 1.什么是Git？

* Git是一个**开源的分布式版本控制系统**，用于敏捷高效地处理任何或小或大的项目。
* Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个**开放源码的版本控制软件**。
* Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了**分布式版本库**的方式，不必服务器端软件支持。

看不懂？没关系，对于小白来说，这些确实比较难理解，不过在初期也没有必要完全弄清楚GIt的原理，就像你学习开车，你也没必要一开始就懂得修车，等以后运用的多了，自然就会有自己的理解。  

在这我先举一个浅显的例子帮助你理解：在日常生活中，在用Word的时候我们肯定遇到过，想要修改内容，但是又不想丢失之前的内容，我们就会用“另存为...”来保存当前文件，但是如果这样的操作多了之后，我们就会发现有一堆名字名字一样的Word，我们很难轻易地分辨哪个是哪个。这时我们就可以使用Git。

Git能帮我们记录每次文件的改动，还可以让同事协作编辑，这样就不用自己管理一堆类似的文件了，也不需要把文件传来传去。如果想查看某次改动，只需要在Github里瞄一眼就可以，岂不是很方便？

---
# 2.安装Git
*Git Bash 其实是一个 Bash，不是 Git。*  
*Git Bash 给我们提供了一个虚拟的 Linux 环境，这样我们就不用忍受 Windows 里面垃圾一般的命令行体验了。*  
*Git Bash 内置了 Git 命令。*  
*上面几句话不理解的话也没关系，用一段时间就明白了。*

&nbsp; 1.[从官网下载](https://git-scm.com/downloads)  
&nbsp; 2.百度网盘:链接:[https://pan.baidu.com/s/1W8jWJZKYGcN30Lx8rE18fQ]( https://pan.baidu.com/s/1W8jWJZKYGcN30Lx8rE18fQ) 提取码: 7amt

双击安装，注意每一步的选项要参考下面的图（如果没有对应的图，就直接下一步）
![](https://user-gold-cdn.xitu.io/2019/2/4/168b6d752f254e65?w=524&h=404&f=png&s=52282)
![](https://user-gold-cdn.xitu.io/2019/2/4/168b6d78512763ec?w=513&h=395&f=png&s=38274)
![](https://user-gold-cdn.xitu.io/2019/2/4/168b6d7ad658800d?w=499&h=388&f=png&s=28441)
![](https://user-gold-cdn.xitu.io/2019/2/4/168b6d7cd9748310?w=502&h=390&f=png&s=31445)  

好了，安装成功。  
## 简单使用：
* 1.在一个目录里空白处，或者在这个目录名字上，右键，选中[Git Bash Here]，即可使用Git Bash 打开这个目录。试试输入`touch 1.txt` 回车，这个目录里是不是多了一个1.txt文件。
* 2.直接打开 Git Bash ，输入 `cd ~/Desktop` 即可来到桌面所在的目录，输入`touch 1.txt` 回车，桌面上是不是多了一个1.txt文件。

---

# 3.配置


## 3.1 配置 Git  
在命令行中输入下面五句话，必须严格的输入，一个空格都不能错，否则git无法使用。
```
git config --global user.email xxxxxx(把xxxxxx替换成你的邮箱跟github一致或者不一致也行)
git config --global user.name xxxxxx(把xxxxxx替换成你的英文名字随便什么都行)
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "vim"
```

## 3.2 配置 GitHub

1. 进入 [https://github.com/](https://github.com/)   注册一个GitHub账号。
2. 进入 [https://github.com/settings/keys](https://github.com/settings/keys)
3. 如果页面里已经有一些 key，就点「delete」按钮把这些 key 全删掉。如果没有，就往下看  
- key：key就是钥匙我们把锁安在我们的GitHub上，每次我们git链接到github时，都会自动的把我们的key插到锁里，匹配的话我们才能成功访问。
- 一台电脑只需要一个 SSH key 。
- 一个 SSH key 可以访问你的所有仓库，即使你有 1000000 个仓库，都没问题。
- 如果你新买了电脑，就在新电脑上重新生成一个 SSH key，把这个 key 也上传到 GitHub，它可以和之前的 key 共存在 GitHub 上。
- 如果你把 key 从电脑上删除了，重新生成一个 key 即可，替换之前的 key。
4. 点击 New SSH key，你需要输入 Title 和 Key，但是你现在没有 key，往下看
5. 打开 Git Bash
5. 复制并运行`rm -rf ~/.ssh/*`把现有的 ssh key 都删掉，这句命令行如果你多打一个空格，可能就要重装系统了，建议复制运行。
6. 运行`ssh-keygen -t rsa -b 4096 -C "你的邮箱"`，注意填写你的邮箱！
7. 按回车三次,不要太快，每次命令行不动了再回车。
8. 运行`cat ~/.ssh/id_rsa.pub`,得到一串ssh-rsa开头的东西，完整的复制这串东西
9. 回到上面第 4 步的页面，在 Title 输入「我的第一个 key」
10. 在 Key 里粘贴刚刚你你复制的那串东西
11. 点击 Add SSH key
12. 回到 Git Bash
13. 运行 `ssh -T git@github.com`，如果询问你（yes/on）？ 输入yes 回车。
14. 然后如果你看到 `Permission denied (publickey). `就说明你失败了，请回到第 1 步重来，是的，回到第 1 步重来；如果你看到` Hi FrankFang! You've successfully authenticated, but GitHub does not provide shell access.` 就说明你成功了！
---

# 4.使用
本文为新手教程，暂时提供三种最常用的使用git的方式：
1. 只在本地使用
2. 将本地仓库上传到 GitHub
3. 下载 GitHub 上的仓库

## 1.只在本地使用
### 1.1  建立版本库(初始化)
*版本库又名**仓库**，英文名**repo**sitory，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。*

1. 首先，找一个合适的位置，创建一个空的目录：`mkdir git-demo-1`
- 干什么都不要在根目录上乱搞，一定要进入一个目录再进行操作，否则系统报废了等着哭吧。
2. 进入该目录：`cd git-demo-1`
- 在windows中，为了避免出现各种莫名其妙的问题，请确保整个目录路径不包含中文。
3. `git init` 把这个目录变成Git可以管理的仓库
- 如果你已经设置了查看隐藏文件，你会在这个目录里看到.git目录。如果没有设置，就输入 `ls -la` 即可看到.git 。  
- `Initialized empty Git repository in C:/Users/Administrator/Desktop/git-demo-1/.git/` 这句提示就是你在`C:/Users/Administrator/Desktop/git-demo-1/.git`已经创建了一个空的仓库。但是请注意，这个仓库不是真的空的，里面还是有一些很复杂文件，所以不要打开看它，看了也看不懂。
4. 我们在 git-demo-1 目录里添加任意文件，比如添加 index.html 和 css/stlye.css  
 ①.`touch index.html`  
②.`mkdir css`  
③.`touch css/style.css`  
5. 运行 `gti status -sb` 我们可以看到  

     `## No commits yet on master`  
 ` ??  css/  `  
`??  index.html`           
??表示Git不知道我们想对这两个文件做什么改动。
-  `git status -sb`：git status 是用来显示当前的文件状态的，哪个文件变动了，方便你进行 git add 操作。-s 的意思是显示总结（summary），-b 的意思是显示分支（branch），所以 -sb 的意思是显示总结和分支。
6. 用命令`git add `将文件添加到[暂存区]  
  ①你可以一个一个地add  
   `git add index.html`  
`git add css/style.css`  
  ②也可以一次性add  
`git add .` 意思是把当前目录（.表示当前目录）里面的变动都加到「暂存区」 
7. 再次运行 `git status -sb` ，这时你可以看到??变成A  
`## No commits yet on master`  
`A  css/style.css`  
`A  index.html`  
A 表示我们告诉Git，这些文件我要添加到仓库。    

8. 使用 `git commit -m "信息"` 将我们 add 过的内容「正式提交」到本地仓库（.git就是本地仓库），并添加一些注释信息，方便日后查阅  
①你可以一个一个地commit  
   `git commit index.html -m '添加index.html'`  
`git commit css/style.css -m "添加css/style.css"`  
  ②也可以一次性commit  
`git commit -m "添加了两个文件"`
9. 再再次运行`git status -sb` ，会发现没有文件变动了，因为文件已经commit到仓库了  
`## master`
10. 这时你使用 `git log` 就可以看到历史上的变动：
```
commit 72cbc14c7a8a0000fb935f31326f15cd12fd6f5b (HEAD -> master)
Author: omnoob <896265004@qq.com>
Date:   Mon Feb 4 16:49:46 2019 +0800

    添加了两个文件

```
11. 以上就是一个完整的git add ; git commit 的过程，多加练习！

### 1.2 文件变动

1. 先修改文件  
  ①我们可以直接到该目录里找到想修改的文件index.html，打开，修改内容。  
  ②我们也可以在命令行中输入 `start index.html` ,会用默认的编辑器打开，修改内容。
2. 把这个改动保存到仓库  
我们可以用上面学习的 git add : git commit 完成。
- 其实，index.html 文件之前已经被我们add过了，所以此处的 add 操作可以省略。但是对于初学者来说，还是不要手懒。也就是说，每一次文件的改动都要执行 git add ; git commit 命令，才能被添加到.git仓库。



## 2.将本地仓库上传到 GitHub

你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作。  
1. 在 GitHub 上新建一个空仓库，名称随意，一般跟本地目录名一致，也叫做 git-demo-1  

- 点击 [new]
![](https://user-gold-cdn.xitu.io/2019/2/4/168b7f471586f2b9?w=1283&h=503&f=png&s=129233)
- 按照截图所示，除了仓库名，其他的什么都别改，这样你才能创建一个空仓库,然后点击 [Create repository]
![](https://user-gold-cdn.xitu.io/2019/2/4/168b7f609425d323?w=802&h=669&f=png&s=65636)
- 如果你用了网页翻译，你会发现GitHub已经说的很明白了，但是复制代码的时候，一定一定要用原网页，否则代码会丢失一些格式,一个空格都不能丢，建议复制，不要手打。  
- 一定要点SSH按钮！因为不点的话默认的是 HTTPS地址，使用 HTTPS 地址很麻烦，因为每次使用 HTTPS 地址都需要输入密码，而 SSH 地址不需要。
![](https://user-gold-cdn.xitu.io/2019/2/4/168b7fa9d95c04cd?w=993&h=766&f=png&s=67124)  
2.因为我们本地已经有了仓库，我们现在是要把本地仓库上传到GitHub上，看图的下半部分，我们一行一行地复制过来执行。  
①找到图中的「…or push an existing repository from the command line」这一行，复制第一行`git remot add origin git@github.com:xxxxxxx/git-demo-1.git`,执行。  
②复制第二行 `git push -u orgin master` ,执行。
③刷新当前页面，你的仓库就上传到GitHub上啦！

## 3.在GitHub上建立一个仓库，然后下载到本地

1.在GitHub上建立一个新的仓库 git-demo-2 ,这一次建一个自带 README 和 Lisence 的仓库  
操作如图所示：
![](https://user-gold-cdn.xitu.io/2019/2/5/168b945f72cb078d?w=779&h=667&f=png&s=41586)

2.仓库里自动拥有三个文件：gitignore、README.md 以及 LISENCE ,至于这三个文件的作用，自己搜索了解。
![](https://user-gold-cdn.xitu.io/2019/2/5/168b94e5a13a5d32?w=1008&h=691&f=png&s=40140)  

3.好了，我们的远程仓库已经建立好了，下面我们使用`git clone`命令把它下载到本地  

如图操作：一定要确保地址是SSH地址！
![](https://user-gold-cdn.xitu.io/2019/2/5/168b95a4409c2422?w=711&h=337&f=png&s=22040)
一定要确保地址是SSH地址！  
4.打开Git Bash，进入一个安全的目录，比如桌面就很安全，`cd ~/Desktop`，运行
5.`git clone 你刚刚得到的git@github.com:开头的地址`，运行。你会发现桌面上多了一个 git-demo-2 的目录。
6.进入这个目录 `cd git0demo-2` ，然后查看文件 `ls-la`,你就会看到远程仓库的文件都在这里了，我们可以在这个目录里添加文件了。先 git add ，再 git commit 。

## 4.总结  
1. git init，初始化本地仓库 .git  
2. git status -sb，显示当前所有文件的状态
3. git add 文件路径，用来将变动加到暂存区
4. git commit -m "信息"，用来正式提交变动，提交至 .git 仓库
5. 如果有新的变动，我们只需要依次执行 git add xxx 和 git commit -m 'xxx' 两个命令即可。
6. git log 查看变更历史
7. git clone git@github.com:xxxx，下载仓库

## 5. 文件更新

你在本地目录有任何变动，只需按照以下顺序就能上传：
1. `git add 文件路径`
2. `git commit -m "信息"`
3. `git pull`  与远程仓库同步，如果有不一样的就更新一下。如果远程仓库没有变化，可以省略此步骤。
4. `git push`  将本地的变动上传到远程仓库。
  
  
---
以上就是我理解的git的新手教程，由于我水平有限，还有很多东西没有写在这里，以后还会补充。
