---
title: Hexo高级功能
date: 2017-02-03 11:51:32
tags: hexo
categories: 博客撰写
toc: true
---
# Next主题设置

参考[站点1](http://www.jeyzhang.com/next-theme-personal-settings.html)，[站点2](http://zhiho.github.io/2015/09/29/hexo-next/)

# 添加文档类别及标签

参考[站点](http://whx4j8.github.io/2016/03/16/hexo-next-%E6%B7%BB%E5%8A%A0%E4%B8%BA%E6%96%87%E7%AB%A0%E6%B7%BB%E5%8A%A0%E5%88%86%E7%B1%BB/)


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

用mathjax添加latex公式可以参考[网站](http://www.jeyzhang.com/how-to-insert-equations-in-markdown.html)，[网站](https://fsh0524.github.io/2016/03/03/LaTeX-in-Hexo/)

用mathjax添加latex公式编号可以参考[网站](http://erofish.github.io/2015/10/14/Hexo%20%E4%B8%AD%E5%88%A9%E7%94%A8%20Mathjax%20%E4%B9%A6%E5%86%99%E5%85%AC%E5%BC%8F/)，[网站](http://www.luohanjie.com/2016-01-28/make-mweb-support-mathjax-automatic-formula-number.html)

公式编号修改代码1

    MathJax.Hub.Config({TeX:{equationNumbers:{autoNumber:"AMS"}}});
    
示例：

$$\frac { dy }{ dx } =\frac { { e }^{ x } }{ 3{ y }^{ 2 } }$$

$$\lim \_{ x\rightarrow \infty  }{ \sum \_{ k=1 }^{ x }{ \frac { \sin { k } +\cos { k }  }{ k }  }  } $$


公式编号修改代码2

    {% if theme.mathjax %}
    <!-- MathJax Start -->
    <!-- MathJax documentation: http://docs.mathjax.org/en/latest/index.html -->
    <script type="text/x-mathjax-config">
      MathJax.Hub.Config({
    tex2jax: {  // tex2jax preprocessor
      inlineMath: [ ['$','$'] ],  // delimiters for in-line math
      displayMath: [ ['$$','$$'] ],  // delimiters for displayed equations
      processEscapes: true,  // enable \$ to represent a single dollar sign
      skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']  // MathJax will not process contents inside these tags 
    },
    TeX: {  // TeX/LaTeX input processor
      equationNumbers: { autoNumber: "AMS" },  // only number those equations in specific AMSmath environments
      extensions: ["AMSmath.js","AMSsymbols.js","noErrors.js","noUndefined.js"]  // introduce AMS extensions and suppress generating error messages 
    },
    "HTML-CSS": {  // HTML-CSS output processor (this is the default output of MathJax)
      scale: 110,  // The scaling factor of math with respect to the surrounding text
      linebreaks: { automatic: true } // automatically breaks the line if necessary
    },
    SVG: {  // SVG output processor
      scale: 110,  // The scaling factor of math with respect to the surrounding text
      linebreaks: { automatic: true } // automatically breaks the line if necessary
    },
    menuSettings: {  // settings for the mathematics contextual menu
      zoom: "Hover"  // set equation zooming to be triggered by a single mouse click
    }
      });
     
      MathJax.Hub.Queue(function() { // Fix <code> tags after MathJax finishes running, this is a hack to overcome a shortcoming of Markdown
      var all = MathJax.Hub.getAllJax(), i;
      for(i=0; i < all.length; i += 1) {
      all[i].SourceElement().parentNode.className += ' has-jax';
      }
      });
    </script>
    <script type="text/javascript"
       src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">  // link to the MathJax CDN
    </script>
    <!-- MathJax End -->
    
    <script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
    {% endif %}


示例：


\begin{equation}
\label{e1}
a = b + c
\end{equation}

参考公式\eqref{e1}


\begin{align} \begin{vmatrix} 1 & 1 & 1 & \cdots & 1 \\ a_{1} & a_{2} & a_{3} & \cdots & a_{n} \\ a_{1}^{2} & a_{2}^{2} & a_{3}^{2} & \cdots & a_{n}^{2} \\ a_{1}^{3} & a_{2}^{3} & a_{3}^{3} & \cdots & a_{n}^{3} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ a_{1}^{n−1} & a_{2}^{n−1} & a_{3}^{n−1} & \cdots & a_{n}^{n−1} \end{vmatrix} = \prod_{1\leqslant{}j < i\leqslant{}n}(a_{i}−a_{j}) \label{eq-multiple} \end{align}

参考公式\eqref{eq-multiple}