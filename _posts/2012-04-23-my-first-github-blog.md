---
layout: post
title: 我在github上的第一篇博客
description: 我在github上的第一篇博客
keywords: life, jekyll
category : life
tags : [life]
---

这是我的第一篇github文章，从周六早上开始搞，已经搞了将近2天了，基本把github的jekyll这套东西就基本明白了。

今晚太晚了，没时间记录下我折腾的过程，明天晚上在公司弄吧！over~

>关于jekyll，网上有个网站[jekyllbootstrap.com](http://http://jekyllbootstrap.com/)不错，不但介绍了jekyll的基本使用方法，还提供了jekyllbootstrap这个快速搭建博客的框架。

>首先，jekyll是个什么东西呢？jekyllbootstrap中讲的很清楚，它不是一个博客软件（像wordpress），而是一个解析引擎。解析什么吗？把我们写的blog(放在_post文件中)，通过_layout中的模板，渲染成一片整体的博客。
这样，我们就只用关心博客的内容了，关于其他的header、评论、footer等等都不需要我们关心。

>其特点：
1. 使用markdown或者textile等标记语言写文章。
2. 可以通过"jekyll --server"命令本地预览博客。
3. 不需要实时的互联网连接，可以写完，等有网络的时候再上传（git的特性了）
4. 可以通过git发布博客
5. 可以放在静态web服务器上。
6. 可以在Github免费host
7. 不需要数据库支持

在我看来，jekyll有4种技术：

Ruby：解析引擎
[Liquid](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)：一种模板语言

    {% for item in array %}
	  {{ item }}
	{% endfor %}
	
YAML：一种数据结构
	
	---
	layout: post
	title: 我在github上的第一篇博客
	description: 我在github上的第一篇博客
	keywords: life, jekyll
	category : life
	tags : [life]
	---

[MarkDown](http://wowubuntu.com/markdown/)：一种标记语言。

详细的奖惩可以看[jekyllbootstrap](http://jekyllbootstrap.com/lessons/jekyll-introduction.html)这篇文章,不懂得，大家可以交流~

