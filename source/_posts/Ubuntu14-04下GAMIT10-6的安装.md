---
title: Ubuntu14.04下GAMIT10.6的安装
date: 2017-03-12 10:03:58
tags: GPS高精度解算
categories: GNSS数据处理
toc: true
---

#安装步骤
将ubuntu切换到root用户权限
```bash
$sudo -s
```

##安装必要软件

```bash
$ apt-get install gcc
$ apt-get install gfortran-4.7
$ apt-get install csh
$ apt-get install tcsh
$ apt-get install libx11-dev
```

**这里需注意:**
 gfortran应该安装4.7版本，根据MIT的教程讲4.8以及版本编号后缀为0的编译器总有bug，建议安装4.7.3。在ubuntu14.04下apt-get可以用gfortran-4.7装之。安装时注意提示缺少什么依赖，就添加其依赖即可。gfortran 4.7安装完成后，由于其执行程序名为/usr/bin/gfortran-4.7，gamit里面编译命令用的gfortran，所以应该将gfortran-4.7建立一个关联，进入/usr/bin/下执行

```bash
$ln -s /usr/bin/gfortran /usr/bin/gfortran-4.7
$ls -la /usr/bin/ | grep gcc 
```

##安装GAMIT

###安装
首先将GAMIT包拷贝到待安装的位置，比如可以在/usr/local/gg/10.6/

进入GAMIT目录，执行

```bash
$./install_software

```

安装过程中会询问一些设置问题，主要会有一个需要修改libraries/Makefile.config文件。
    X11LIBPATH /usr/lib/x86_64-linux-gnu
    x11INCPATH /usr/include/X11

###配置GAMIT环境变量

修改.bashrc文件，加入下列变量
	export PATH="$PATH:/usr/local/gg/10.6/gamit/bin:/usr/local/gg/10.6/com:/usr/local/gg/10.6/kf/bin"
	export HELP_DIR="/usr/local/gg/10.6/help/"
编辑完后执行

```bash
source ~/.bashrc
```

###验证是否安装成果
在term中执行
```bash
$doy
$sh_get_rinex
```
无法正确返回相应内容，请检查环境配置步骤中的内容是否正确，缺少符号。

**HELP_DIR不要掉了最后一个反斜杠，PATH不要掉了com目录**