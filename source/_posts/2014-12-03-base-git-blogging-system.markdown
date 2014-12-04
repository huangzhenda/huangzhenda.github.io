---
layout: post
title: "利用Octopress搭建博客"
date: 2014-12-03 17:03:25 +0800
comments: true
categories: Octopress
tags: [Octopress,搭建博客,Github]
keywords: octopress, 搭建博客，Github,Github Page Service
decription: 如何利用Octopress在Github搭建自己的博客

---

首先很感谢以下几位作者，文章都写得很好，对我的帮助很大。

1. [象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)
2. [Octopress - 像黑客一样写博客](http://williamherry.com/blog/2012/07/20/octopress-setup/)
3. [让Octopress博客在多台Mac上同时使用](http://foggry.com/blog/2014/04/02/ru-he-pei-zhi-rang-ni-de-octopressbo-ke-zai-duo-tai-macshang-tong-shi-shi-yong/)
4. [自定义你的Octopress博客](http://foggry.com/blog/2014/04/28/custom-your-octopress-blog/)
5. [破船之家：利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/)


###目录
* Octopress介绍
* Octopress安装
* Octopress配置
* 部署博客到HubGit
* 写博客
* 小结

---
###Octopress 介绍
Octopress网址： [http://octopress.org/](http://octopress.org/)

Octopress是建立在Jekyll博客引擎的一个博客系统，能够为你生成html模块，css样式，javascript,并且能够自定义配置自己的博客。号称是hacker专属的博客系统（a bolgging framework for hackers）。

<!---more--->

---
###Octopress安装

安装过程主要经过几个步骤：git安装->ruby安装->从github clone octopress->安装octopress依赖项->安装octopress默认主题。

######1. git安装
  Octopress是基于 ```git``` ，所以在使用Octopress前，你应该能够知道一些shell以及git的基本操作。所以第一步就是确保你已经安装了Git。
 <pre>
  $ git --version
   git version 1.9.3 (Apple Git-50)
 </pre>
 利用上面的命令,判断是否已经安装。如果没有，请先安装Git。
 
   [Git安装](http://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)
 
#####2.ruby安装
   Octopress要求 ruby 1.9.3 or greater,也就是说至少要在ruby 1.9.3以上。
    安装ruby有两种方法，RVM和rbenv，这里介绍的是rebenv的方法，如果要使用RVM的话，请自己去搜索。
   
  （1）第一步先确定是否有安装rbenv (mac系统本身就已经安装了ruby)
  
   ```
   rbenv --version #这边如果有安装的话，会有响应rbenv 0.4.0
   ```
   (2) 如果没有，下载安装rbenv
   
   ```
   $cd 
   $git clone git://github.com/sstephenson/rbenv.git .rbenv
   ```
   
   (3) 将```~/.rbenv/bin```加到你的```$PATH```变量里以使你可以使用rbenv这个命令。

   ```
   echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
   source ~/.bash_profile 
   ```

  (4) 添加 rbenv init到你的shell以启用shims和自动补全功能

  ``` 
  echo 'eval "$(rbenv init -)"' >> ~/.bash_profile 
  ```
  
  (5) 启动 shell让 ```$PATH``` 的修改 生效，现在可以使用 

  ```
  rbenv了exec $SHELL	    
  ```
  (以上rbenv安装是参考：[Octopress - 像黑客一样写博客](http://williamherry.com/blog/2012/07/20/octopress-setup/),或者 octopressg网站教程：[http://octopress.org/docs/setup/rbenv/](http://octopress.org/docs/setup/rbenv/))
  
  (6) 这时候，我们就可以用rbenv安装ruby了。

  ```
  $rbenv install 1.9.3-p0
  $rbenv rebash
  ```
  (7) 再次确定ruby是否安装
  
	e>$ruby --vresion  #这个时候应该会响应（ruby 1.9.3）
 
#####3.clone Octopress
Octopress的源码是放在Github上，所以我们只要将Otopress克隆到我们本地上就可以了。
	
	$git clone git://github.com/imathis/octopress.git octopress
	$cd octopress 
#####4.安装依赖项

	$gem install bundler
	$rbenv rehash    # If you use rbenv, rehash to be able to run the bundle command
	$bundle install
	
#####5.安装默认的Octopress主题
	rake install
	
这个时候，我们这个Octopress的安装已经完成了，这时候，你可以打开你
 
 ![Alt Octopress目录结构](/images/octopress/octopress_mulu.png)
 
* _config.yml与 Rakefile是属于配置文件
* source: 工作区目录
* public: 存放本地的生成的博客。
* _deploy: 这个目录你目前还没有，这个目录会在后面操作做，自动生成的，它包含了所有要被部署到远程仓库的新仓库。所指向的是 ```master```分支。
* sass:


- - - - 
###部署博客到Github
 Octopress可以将博客部署到Github,Heroku,和自己的主机上。这里介绍的是部署到Github上。

1. GitHub可以免费托管的Git社区。GitHub的Page Service可以免费托管博客，并且可以自定义域名。
那么首先要先到GitHub上注册个账号。<https://github.com>
2. 注册成功后，创建一个 username.github.io的资源库（repositories）。
3. 创建好Github仓库后，现在在终端输入  
	
	```
 	$rake setup_githut_pates
	```
4. 执行后，就会上面说到的 ```_deploy``` 目录，这个亩留主要是存放部署到master分支的内容的。这期间会要求输入你 Github仓库的地址url。
5. 完成上面命令后，继续执行下面命令：	
	
	```
	$rake generate
	$rake deploy
	```
   * rake generate,会根据source目录下的内容，生成静态文件，并且存放到publice/目录下
   * rake deploy，会将public/目录下的内容覆盖_deploy/目录下，然后将_deploy/目录下的修改pull到gitHub的master分支上。
6. 现在你可以你的Github上```username.github.io```仓库，就可以看到已经上传了一个内容。打开浏览器，输入 ```username.git.io```，就可以看到你的博客首页了。
7. 别以为这样就结束了，同时，你也要把octopress，pull到github，我们把他放到source分支下。
	
	```
	$git add .
	$git commit -am "Some comment here."
	$git push origin source #update the remote source branch
	```
8. 这样你就完成了整个博客的部署。如果你想在公司的电脑上，家里的电脑，其他的电脑都能写我们的博客，可以参考 [让Octopress博客在多台Mac上同时使用](http://foggry.com/blog/2014/04/02/ru-he-pei-zhi-rang-ni-de-octopressbo-ke-zai-duo-tai-macshang-tong-shi-shi-yong/)
9. 最后，每次修改前，为了防止冲突，最好先做下更新。 可执行以下命令分别更新source 和 master分支。

	```
	$cd octopress
	$git pull origin source #update the local source branch
	$cd ./_deploy
	$git pull origin master #update the local master branch
	```

----
### 写博客
1. 新建一篇博客
	
	```
	$rake new_post["title"]
	```
	执行后，会在```/source/```下生成```_posts```目录，并且按照Jekyll的命令规范对文章进行命令： YYYY-MM-DD-post-title.markdown。
2. 打开编辑 YYYY-MM-DD-post-title.markdown。博文采用的markdown语法，所以不熟悉的小伙伴，最好去了解。
3. 编辑好博客后，我们发布我们的博文了。
	
	```
	$rake generate
	$rake deploy
	```
4. 最后还需要把修改后的source分支pull到Github上。

	```
	$git add .
	$git commit -am "Some comment here"
	$git push origin source
	```
5. 其他的命令，比如
*rake preview ：可以预览博客，命令输入后，就可以在浏览器，输入localhost:4000 打开预览。

----
### 小结

  对于博客的其他的一些配置，主题美化，希望能在后期的不断完善。同时对于Git不了解的，刚好发现了一个很好的教程 [廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) 
  
   基本上一个博客就可以这样搭建起来，但是一个博客最主要还是博文，要写出好的博文，是需要花精力的。好记性不如烂笔头，写博文，不仅能将自己理解的东西分享给别人以外，同时还能整理思路。我们现在面对太多碎片化的信息，往往都是看过后，很快就会忘记。写博客能够很好的帮助你整理，保存，记忆东西。
  
