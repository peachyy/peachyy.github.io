---
layout: post
title: github pages + jekyll +markdown 搭建博客教程
categories : [技术]
tags : [GitHub,jekyll,markdown] 
---

现在很多文章都是使用免费而且无流量限制的github pages托管，下面将一步一步的描述整个过程。

 *这种博客系统是基于Jekyll，所以就得安装Ruby了。*

1、安装Ruby 相关。  
	`Ruby`可以从它的[https://www.ruby-lang.org/en/downloads/](https://www.ruby-lang.org/en/downloads/ "官方站点") 下载到，国内有时候网络不稳定很难下载成功，可以在[http://wd.jb51.net:81/201403/tools/ruby_win64.rar](http://wd.jb51.net:81/201403/tools/ruby_win64.rar "下载") 下载到安装包，然后安装到操作
	系统。
  
	安装Devkit工具包，首先进入到devkit的安装目录
	{% highlight ruby %}
	ruby dk.rb init//会在该目录生成一个`config.yml`
	
	ruby dk.rb install//安装工具命令行
	
	gem instal jekyll //安装 jekyll 
	
	//测试jeykyy 是否安装成功。返回版本号则说明我们安装成功了。
	jekyll -v
	
	
{% endhighlight %}
   设置`ruby`的path 把ruby的安装目录下的`bin`文件夹加入系统变量`path`中，如下图：
	![](http://i.imgur.com/bTgOA5N.png)

2、安装python 使用python的 `easy_install`
	安装python略过
	//安装代码高亮插件
	{% highlight ruby %}

	{% endhighlight %}

========================================