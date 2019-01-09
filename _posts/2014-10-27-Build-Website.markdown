---
layout: post
title:  "搭建个人站点"
categories: Web
tags: "学习笔记" 
---
### 使用GitHub ###

安装git工具：
[GitHub for Windows](https://windows.github.com/)

创建个人站点：
详见[后文链接](#ref)

### 搭建Jekyll ###


1.下载并安装[Ruby](https://www.ruby-lang.org/)

2.下载并安装[DevKit](http://rubyinstaller.org/downloads/)

    ~ $ cd C:\DevKit
    ~ $ ruby dk.rb init
    ~ $ ruby dk.rb install

3.安装Jekyll并检测是否安装成功

	~ $ gem install Jekyll
	~ $ jekyll --version

4.安装Rdiscount

	~ $ gem install rdiscount

5.运行本地工程

cd至工程目录，启动服务

	~ $ jekyll new my-awesome-site
	~ $ cd my-awesome-site
	~ $ jekyll server
	# => Now browse to http://localhost:4000


### 编码问题 ###

Windos下用Editplus另存为时候能够选择编码，改成UTF-8编码即可。

<h3 id="ref">参考链接</h3>

- [通过GitHub Pages建立个人站点（详细步骤）](http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html)
- [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
- [Jekyll官网](http://jekyllrb.com/)
