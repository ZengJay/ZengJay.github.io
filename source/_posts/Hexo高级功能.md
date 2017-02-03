---
title: Hexo高级功能
date: 2017-02-03 11:51:32
tags:
---
# 添加文档类别及标签

# 添加图片
## 配置
首先，在 \_config.yml 中设置
    post_asset_folder: true
注意：该选项一般在默认的\_config.yml中有，如图
![](/hexo-image-config.jpg)
## md文件中添加
使用hexo-asset-image插件插入图片

首先,确认 _config.yml 中有 post_asset_folder:true 。

其次，在 hexo-blog 目录，执行

    npm install hexo-asset-image --save

假设在

    Hexo高级功能
    ├── hexo-image-config.jpg
	└──
    Hexo高级功能.md

这样的目录结构（目录名和文章名一致）中，只要使用` ![](/hexo-image-config.jpg)` 就可以插入图片。

参考[here](http://supermaryy.com/2016/07/02/)

# 添加latex数学公式

