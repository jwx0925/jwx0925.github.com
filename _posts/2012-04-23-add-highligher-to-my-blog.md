---
layout: post
title: 使用SyntaxHighlighter为博客添加代码高亮功能
description: 使用SyntaxHighlighter为博客添加代码高亮功能
keywords: syntaxHighlighter, highlighter,blog
category : life
tags : [life]
---

作为一个新时代的码农，写博客多数是技术性的blog，所以少不了写代码，如果没有高亮支持的代码显示，读有代码的blog是很不爽的一件事。所以，今天开始为我的博客寻找支持代码高亮的插件。
Google一下，找到了[《9款有用的Javascript代码高亮脚本》](http://www.qianduan.net/9-useful-javascript-syntax-highlighting-scripts.html)这篇文章，选择了第一个插件[SyntaxHighlighter](http://alexgorbatchev.com/SyntaxHighlighter).

1. [下载](http://alexgorbatchev.com/SyntaxHighlighter/download/)SyntaxHighlighter包，我下载的是最新的3.0.83版本。
2. 跟着例子做：
    添加shCore.js、shCore.css和shThemeDefault.css（也可以选择其他的theme.css）到你的page
	
			<script type="text/javascript" src="js/shCore.js"></script>
			<link href="css/shThemeDefault.css" rel="stylesheet" type="text/css" />
			<link href="css/shThemeDefault.css" rel="stylesheet" type="text/css" />
			
	添加你需要的语言的js，比如javascript的是css/shBrushJScript.js
	
			<script type="text/javascript" src="css/shBrushJScript.js"></script>
	
	添加如下js代码：
	
			<script type="text/javascript">
				 SyntaxHighlighter.all()
			</script>

	完成！
			
3. 在页面上如何使用？有两种方法：
	第一种：
    
    	<p re class="brush: js">
    		/**
    		 * SyntaxHighlighter
    		 */
    		function foo()
    		{
    			if (counter <= 10)
    				return;
    			// it works!
    		}
    	</pre>
		
第二种（PS：一定要加上CDATA）：

    	<s cript type="syntaxhighlighter" class="brush: js"><![CDATA[
    	  /**
    	   * SyntaxHighlighter
    	   */
    	  function foo()
    	  {
    		  if (counter <= 10)
    			  return;
    		  // it works!
    	  }
    	]]></script>
	
PS:为了让其显示原版，我把< pre>和< script>都写错了。。。
    
    最终的效果为：
	<pre class="brush: js">
		/**
		 * SyntaxHighlighter
		 */
		function foo()
		{
			if (counter <= 10)
				return;
			// it works!
		}
	</pre>
啊！基本上是翻译的SyntaxHighlighter的文档。水了水了~~