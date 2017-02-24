---
title: 十步之内轻松搞定 Hexo + GitHub搭建个人博客
date: 2017-02-25 00:24:53
categories: 配置安装教程
tags: [Git,GitHub,GiHub Pages,Hexo,Blog,.md,Markdown,Hexo主题,Hexo配置,Hexo指令,Yelee,MacOS安装Git,brew,MacOS安装Node,npm,Window安装配置Git,Window安装配置node,Hexo写博客,GitHub搭建个人站点]
---
最近刚把个人博客站点搭起来，应几个朋友之邀，同时也是趁着自己还没忘，赶快把搭建过程记录分享一下。因为GitHub我一直有在用，所以个人感觉没有很复杂。如果你对细节没有太多要求的话，跟着这个教程，差不多个2个小时左右可以轻松搞定。如果，你想去仔细研究配置和主题的东西，半天时间下来也差不多可以上线。所以，总的来看就是这么个工作量。具体的，我们接下来慢慢聊。  

<!-- more -->  
*** 
### 包含的所有技术关键词 以及 简单介绍  
**[Git](https://git-scm.com/)**: 其实说白了就是帮你管理代码，进行版本控制。让你life easier的东西。官网是这么介绍自己的:  
> Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.  
  
如果你没用过Git, 官方有一个在线的bash 请戳 -> [https://try.github.io/levels/1/challenges/1](https://try.github.io/levels/1/challenges/1)  
拢共大概十个命令，日常工作也完全是够用了。更高级的用法请自行Google...  
  
**[GitHub](https://github.com/)**: 我也不瞎编了, 人家维基百科[3]是这么说的：  
> GitHub是一个通过Git进行版本控制的软件源代码托管服务。GitHub同时提供付费账户和免费账户。这两种账户都可以创建公开的代码仓库，但是付费账户还可以创建私有的代码仓库。根据在2009年的Git用户调查，GitHub是最流行的Git访问站点。除了允许个人和组织创建和访问保管中的代码以外，它也提供了一些方便社会化共同软件开发的功能，即一般人口中的社区功能，包括允许用户追踪其他用户、组织、软件库的动态，对软件代码的改动和bug提出评论等。GitHub也提供了图表功能，用于概观显示开发者们怎样在代码库上工作以及软件的开发活跃程度。  
   
**Hexo**: 是一个快速，简洁且高效的博客框架 [中文官方网站](https://hexo.io/zh-cn/)  
  
**[Markdown](http://www.markdown.cn/)**: 所有博客原始文件都是 **.md** 文件。都是用它来写的。好吧，如果你知道 **txt** 的话，其实他们是一类玩意儿。  
> Markdown 是一种轻量级标记语言，创始人为約翰·格魯伯（John Gruber）。 它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”。    
  
***  
画个分界线  - 教程正式开始  
***  
  
## 1. 如果你用MacOS, 请先安装一下 brew  
```bash  
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"    
``` 
## 2. 安装 Git  
```bash  
brew install git  
```  
Windows下安装配置Git请参考: [Win7上Git安装及简单配置过程](https://my.oschina.net/u/998304/blog/353386)  
  
## 3. 注册GitHub 获取并上传本机SSH 新建一个代码库  
##### 3.1 注册GitHub
---  
[登录官网](https://github.com)，一步一步往下点就行了  
{% asset_img 1Github.png 注册GitHub%}  
  
##### 3.2 获取并上传本机SSH
---  
关于这一步, GitHub上面有很清晰的指导   -> [链接地址](https://help.github.com/articles/connecting-to-github-with-ssh/)  
为了本文的完整性，我截几个图放这里:  
**3.2.1 首先查看自己电脑里是否已经有了SSH key, 如果有了直接上传就行**  
```bash  
ls -al ~/.ssh  
```  
**3.2.2 如果还没有, 你需要执行以下几步**  
{% asset_img 3.2.2.1GenerateSsh.png Generate SSH%}  
{% asset_img 3.2.2.2AddAgent.png Add SSH-Agent%}  
**3.2.3 上一步成功之后**  
去你的本地找到以下文件，拷贝里面的内容 **注意不要前后空格**  
{% asset_img 3.2.3.1LocalSSH.png Local SSH%}  
然后回到, GitHub, 到以下页面进行添加  
{% asset_img 3.2.3.2.png 添加SSH%}  
{% asset_img 3.2.3.3.png 添加SSH%}  
  
##### 3.3 新建一个代码库  - 这个略关键  
---  
首先，GitHub有个功能叫 **GitHub Pages**。这个也是本教程的**关键**。你可以使用GitHub Pages直接从代码库生成个人站点，运行之后，直接通过GitHub提供的域名（用户名+github+io)进行访问。但是，为了使用这个功能，我们必须符合他们的规范: 新建Repository时, name 对应处填写资源名，需要使用自己的用户名，**每个用户名下面只能建立一个**，资源命名 **必须符合这样的规则username/username.github.io**  

**3.3.1 首页选择新建Repository**  
 
{% asset_img 3.3.1.png 新建Repository%}  
**3.3.2 项目名称设置为 yourname.github.io 并clone到本地**  
  
{% asset_img 3.3.2.png yourname.github.io%}  
**3.3.3 项目创建成功之后，去项目页面，点击settings**  
  
**页面往下拉，找到 GitHub Pages 分支选择master 并保存**
**大概过10分钟左右，你的页面就可以直接通过yourname.github.io进行访问了**  
{% asset_img 3.3.3.png 新建Repository%}  
{% asset_img 3.3.4.png 新建Repository%}   

## 4. 重点来了 - 安装 Hexo
开始之前, 请确保你的本机成功正确的安装了Node。  
如果没有, 如果你用Mac, 同样一键傻瓜式安装:  
```bash    
brew install node  
```  
Windows 同样请参考  ->  [Node.js 安装配置](http://www.runoob.com/nodejs/nodejs-install-setup.html)  
  
其实吧, **Hexo** 安装起来超级简单, 就一行命令就行:  
```bash  
$ npm install hexo-cli -g  
```  
## 5. 继续划重点 - 初始化 Hexo  
先建个文件夹  
```bash  
mkdir HelloBlog  
```  
进入文件夹, 执行命令:  
```bash  
hexo init  
npm install  
```  
然后等待 下载安装一大堆东西 ...  大概应该是这个样子的  ...  
{% asset_img 5hexoInit.png hexo init%}  
  
这两步完成之后, 当前目录下, 继续执行命令:  
```bash  
hexo generate   或者是  hexo g  
```  

然后, 执行:  
```bash  
hexo server    或者是  hexo s  
```  
{% asset_img 5hexoserver.png hexo server%}  
  
你就会发现, 本地的Blog已经跑起来了...  
{% asset_img 5run.png 效果%}  
## 6. 重点中的重点 - 修改配置文件并部署到GitHub  
首先来看一下, 初始化项目的目录结构长什么样子...  
{% asset_img 6.1.png 目录结构%}  
我们需要修改的就是根目录下的这个 **_config.yml** 顺嘴提一句, 博客的原始文件会存在source/_posts/ 下面  
  
用 **Sublime** 或者 **Atom** 打开文件进行修改  
关于配置文件的具体信息, 可以参考 -> [https://hexo.io/zh-cn/docs/configuration.html](https://hexo.io/zh-cn/docs/configuration.html)  
  
首先是改一下Blog的基本信息:  
{% asset_img 6.2.png %}  
  
最重要的是添加部署的信息, **deploy**这里只用替换repo就行了, 指向你在GitHub里新建的那个库  
{% asset_img 6.3.png Deploy信息%}  

修改完, 保存之后, 根目录下执行:  
```bash  
hexo deploy		或者		hexo d  
```  
如果一切顺利的话, 就可以成功部署到GitHub上了  
{% asset_img 6.4.png Deploy信息%}   
等几分钟就可以通过 -> [gavinlee1.github.io](gavinlee1.github.io) 进行访问了  
## 7.你感兴趣的 - 关于主题的事情  
Hexo有很多开源的主题和插件可以添加配置。以本博客为例, 我用的主题叫 [Yelee](https://github.com/MOxFIVE/hexo-theme-yelee) 可以在github上找到源码和对应的wiki介绍。  
添加个人主题, 主要有这么几步:  
##### 7.1 在 theme 目录下 git clone 你想要的主题  
{% asset_img 7.1.png Deploy信息%}  
##### 7.2 修改全局配置文件 (/_config.yml) - 替换主题  
{% asset_img 7.2.png Deploy信息%}  
##### 7.3 配置和定制主题的显示内容  
你可以在 [https://hexo.io/themes/](https://hexo.io/themes/) 这里找到你想要的主题。但是不同主题有自己的配置信息结构和修改方法, 具体需要参考你所选主题在GitHub上的wiki。当然, 万变不离其宗的是修改**主题目录下的(_config.yml)**  
  
以我用的主题为例 [Yelee 主题使用说明 [简中] ](http://moxfive.coding.me/yelee/)  官方提供完整的配置使用说明书。你可以参考该说明, 修改配置文件, 字体, 高亮格式 以及 包括分享 评论 在内的各种插件。  
## 8.怎么写博客？  
执行命令:  
```bash  
hexo new post "This is the tile for new post."  
```  
这个命令会在 /source/_post/ 下生成 title.md 文件。这个**.md**文件就是你博客的原始文件。编辑完之后，hexo会解析这个文件生成对应的静态网页。然后执行命令进行部署:  
```bash  
hexo clean && hexo generate && hexo deploy  
```  
# 还等什么？ 赶快搭建你自己的Blog咯

 
  
  



### References  
[1] Git [https://git-scm.com/](https://git-scm.com/)  
[2] Hexo [https://hexo.io/zh-cn/](https://hexo.io/zh-cn/)  
[3] 维基百科 - GitHub [https://zh.wikipedia.org/wiki/GitHub](https://zh.wikipedia.org/wiki/GitHub)  
[4] [Win7上Git安装及简单配置过程](https://my.oschina.net/u/998304/blog/353386)  
[5] [Node.js 安装配置](http://www.runoob.com/nodejs/nodejs-install-setup.html)  
[6] [Yelee 主题使用说明 [简中] ](http://moxfive.coding.me/yelee/)