---
title: scidb用法
date: 2017-11-27 00:04:47
tags: scidb
categories: 大数据计算
toc: true
---

### What is scidb? ###

* array-based data manage
* scale up


### How do I install scidb? ###

#### 操作系统

* 安装OSGeolive 10.5
  1.虚拟机只有虚拟硬盘，将名字改为osgeo-live-10.5去掉“-vm”。
  2.在vm中建立新虚拟机，新建虚拟机的所在目录与OSGeolive的官网虚拟机虚拟硬盘所在目录在一个目录即可。新建虚拟机名字为“osgeo-live-10.5”，和官网虚拟机的名字一样（去掉-vm后），这样就可以建立一个vm中直接运行的OSGeolive的虚拟机了。虚拟机硬盘选择

#### 通过docker来安装  

* 安装docker engine
  
  1.将虚拟机的硬盘扩展容量，
  
  2.docker engine安装空间
  
  https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-docker-ce
  
  
  或者 安装docker CE 版
  
  https://www.docker.com/get-docker
  
* 安装scidb-eo


* 从host机器登陆scidb-eo容器（instance）

ssh -p 33330 scidb@localhost
密码 xxxx.xxxx.xxxx

  
* 安装R 3.3

  ubuntu 14.04 自带R是3.02版本，而rgdal需要3.3以上版本
  
  https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04
  
  镜像选择清华大学的镜像，注意：按照上面这个网址将key加入才可以安装新版R
  
  https://mirrors.tuna.tsinghua.edu.cn/CRAN/


### scidb基本配置

* host主机采用OSGeolive 10.5

* 容器docker中安装scidb

* host 主机
  
### scidb 远程 client访问

scidb的主机地址为172.17.0.2 端口为8080

scidbconnect(host = 172.17.0.2, port =8080)


带gdal的scidb driver在client端远程操作

	```R
	gdal_translate(src_dataset = datasets[1],
				   dst_dataset = "SCIDB:array=MOD13A3 host=172.17.0.2 port=8080", of = "SciDB", a_srs = wkt,
				   co = list("t=2000-01", "dt=P1M", "type=STS"))
	```