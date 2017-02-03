---
title: 用hexo构建自己的博客
date: 2017-02-03 11:39:02
tags: hexo
toc: true
---

## 必要软件的安装

### 需要安装的软件
+ nodejs
+ git(如果打算把整个hexo的源码都用github来管理的话，最好用1.9版本)
+ github

### hexo的安装
在nodejs的cmd下安装

···bash
$ npm install -g hexo
$ npm install hexo-generator-index --save #索引生成器
$ npm install hexo-generator-archive --save #归档生成器
$ npm install hexo-generator-category --save #分类生成器
$ npm install hexo-generator-tag --save #标签生成器
$ npm install hexo-server --save #本地服务
$ npm install hexo-deployer-git --save #hexo通过git发布（必装）
$ npm install hexo-renderer-marked@0.2.7--save #渲染器
$ npm install hexo-renderer-stylus@0.3.0 --save #渲染器
···

### 软件使用

+ git命令用github的shell，进入blog的源码目录来输入

+ hexo命令用nodejs的cmd，进入blog的源码目录来输入

## 本地构建hexo的博客
通过nodejs的cmd下建立一个目录hexo-blog。进入该目录后
···bash
$ hexo init
···
参考[这里](https://xuanwo.org/2015/03/26/hexo-intor/)，[这里](http://www.jianshu.com/p/017e01718d41)。

### 建立新页面

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)


### 生成静态文件

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### 运行本地服务

``` bash
$ hexo server
```
本地hexo已建立成功，测试网址：http://localhost:4000

More info: [Server](https://hexo.io/docs/server.html)

## 用github发布并管理博客

### 构建repository
+ 登录github
+ 建立名为zengjay.github.io的repo
### 本地部署配置
+ 保证hexo-blog目录下的_config.yml文件deploy有如下格式：
    
	deploy:
  		type: git
 		 repo: git@github.com:ZengJay/ZengJay.github.io.git
 		 branch: master

**[注意：]()**
[**type、repo、branch的冒号后面必须有空格**](#),如果type后没空格，运行hexo d不会有反应。

### 只保留hexo的源码在一台本地机器的部署方法
+ 在本地hexo博客源码目录hexo-blog下将博客部署到远程站点

  ``` bash
   $ hexo deploy
  ```

   More info: [Deployment](https://hexo.io/docs/deployment.html)

### 在不同机器上对hexo的源码进行版本管理

+ [**在建立好博客的本地源码后，不要部署到github的zengjay.github.com上。**]()
+ 在hexo-blog目录下构建git的master分支与hexo分支。master分支用来管理博客网页；hexo分支用来管理hexo的源码。
+ 构建git的master分支

``` bash
    echo "# zengjay.github.io" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin https://github.com/ZengJay/zengjay.github.io.git
    git push -u origin maste
```

+ 构建git的hexo分支

``` bash   
    git checkout -b hexo //创建并切换到hexo分支
    git add .
    git commit -m "hexo initial commit"
    git push -u origin hexo
```

+ hexo部署

``` bash
    hexo d
```

+ 新机器的管理

## 常见问题及解决方法

### github的ssh公匙问题

1. 无法认证


2. 无法联通github

安装低版本（1.9）的git，可以到[这里](https://github.com/msysgit/msysgit/releases/tag/Git-1.9.5-preview20141217)下载。
