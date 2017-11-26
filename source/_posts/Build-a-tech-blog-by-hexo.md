---
title: 用hexo构建自己的博客
date: 2017-02-03 11:39:02
tags: hexo
categories: 博客撰写
toc: true
---

## 必要软件的安装

### 需要安装的软件
+ nodejs
+ git(如果打算把整个hexo的源码都用github来管理的话，最好用1.9版本)
+ github

### github的密匙配置

确认windows下的当前用户名的目录下没有.ssh目录

### 生成SSH Key
```bash
ssh-keygen -t rsa -C "your_email@example.com" 
```
提示会让输入保存密匙的文件名(含路径)。

生成SSH KEY的时候还要求输入私钥密码"│Enter passphrase (empty for no passphrase):"。如果输入一定请记住私钥的密码， 下面导入私钥的时候要用到；如果不输入的话，直接回车即可。

将SSH 私钥增加到ssh-agent:

```bash
 ssh-add ~/.ssh/xyz_rsa
```

这里会提示输入一次私钥的密码，查看已经add的SSH KEY： 
```bash
 ssh-add -l；
```
### github添加密匙
浏览器登录自己的github页面, 进入"Account Settings", 再点击左边的"SSH Key"可以看到自己上传过的SSH公钥列表. 再点击"Add SSH Key"新增一个公钥, 直接将上面步骤中生成的密匙粘贴即可；

### 测试是否能联通github
测试SSH Key登录
```bash
ssh -T git@github.com
```

如果有问题可以通过命令
```bash
ssh -v git@github.com
```
来查看问题信息

### hexo的安装
在nodejs的cmd下安装

```bash
$ npm install -g hexo
$ npm install hexo-generator-index --save #索引生成器
$ npm install hexo-generator-archive --save #归档生成器
$ npm install hexo-generator-category --save #分类生成器
$ npm install hexo-generator-tag --save #标签生成器
$ npm install hexo-server --save #本地服务
$ npm install hexo-deployer-git --save #hexo通过git发布（必装）
$ npm install hexo-renderer-marked@0.2.7--save #渲染器
$ npm install hexo-renderer-stylus@0.3.0 --save #渲染器
```

### 软件使用

+ git命令用github的shell，进入blog的源码目录来输入

+ hexo命令用nodejs的cmd，进入blog的源码目录来输入

## 本地构建hexo的博客

通过nodejs的cmd下建立一个目录hexo-blog。进入该目录后

```bash
$ hexo init
```

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
		git push -u origin master
	```

+ 构建git的hexo分支

	``` bash   
		git checkout -b hexo //创建并切换到hexo分支
		git add .
		git commit -m "hexo initial commit"
		git push -u origin hexo
	```

+ 生成静态文件并部署到master分支

	``` bash
		hexo g
		hexo d
	```

+ 更新hexo源码后，提交到github

	```bash
	   git add .
	   git commit -m "更新内容"
	   git push -u origin hexo
	```


+ 另一新机器的hexo源码部署

首先，将hexo源码clone到新机器某目录下

	```bash
		git clone -b hexo https://github.com/username/username.git [新机器hexo源码目录]
	```

然后，在source目录下更改博客内容md文件。

完成后可以用hexo g生成博客网页，hexo d部署博客网页。

并通过下面git操作将博客源码文件（分支hexo）提交到github。
	```bash
    // 添加源文件
		git add .
		// git提交
		git commit -m ""
		// 先拉原来Github分支上的源文件到本地，进行合并
		git pull origin hexo
		// 比较解决前后版本冲突后，push源文件到Github的分支
		git push origin hexo
	```
1. 新机器上变更md文件，最好只将md文件提交到github
2. 新机器上最好只hexo g与hexo s，不要运行hexo d，hexo d将生成的博客站点文件git推到github的博客站点的master分支上去了。
3. hexo d 只由最原始的机器上布设？？

新机器部署hexo源码提交时，github上需要提交本机器的密匙。
1. windows下进入User/Adminstrator/.ssh目录（linux则是在~/.ssh下），如果该目录下有id_rsa.pub，有密匙无需重新建立，没有则运行如下命令建立密匙

	```bash
	$ ssh-keygen -t rsa -C "your@email.com"
	```
2. 登陆github保存hexo源码的用户，账户的设置中，为本机添加SSH key
3. 将ssh-rsa.pub中的内容copy到github账户设置中的SSH key中去

	
参考[here](https://sawyersun.github.io/2017/01/20/Using-git-branch-manage-hexo-blog/)，[here](http://www.jianshu.com/p/6fb0b287f950)



+ 版本控制和持续集成

参考[here](https://formulahendry.github.io/2016/12/04/hexo-ci/#more)

## 常见问题及解决方法

### github的ssh公匙问题

+ 无法认证github的密匙

运行

	ssh -T git@github.com

出现如下问题

	```bash
		Warning: Permanently added "github.com,*.*.*.*"(RSA) to the list of known hosts.
		Permission denied(public key). 
	```
原因：

安装了github for windows，其ssh配置文件中认证文件为~\.ssh\github_rsa，ssh-keygen生成的不是此文件，因此，可以重装github，并把github_rsa文件的密匙添加到github网站个人信息中即可。
解决方法：

详细请参考[网站](http://www.cnblogs.com/zjrodger/p/3952692.html)


+ 无法联通github

出现如下问题
    Permission denied (publickey). fatal: Could not read from remote repository.
    Please make sure you have the correct access rights and the repository exists.

如图所示：
![](\public-key-access-right.jpg)

卸载git高版本，然后安装低版本（1.9）的git，可以到[这里](https://github.com/msysgit/msysgit/releases/tag/Git-1.9.5-preview20141217)下载。

参考[网站1](https://github.com/hexojs/hexo/issues/1495),[网站2](http://lijialalala.github.io/2016/04/05/hexoxo-usage/),[网站3](http://www.cognize.me/2015/08/22/msysgiterror/)
