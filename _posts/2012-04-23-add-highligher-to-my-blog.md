---
layout: post
title: ʹ��SyntaxHighlighterΪ������Ӵ����������
description: ʹ��SyntaxHighlighterΪ������Ӵ����������
keywords: syntaxHighlighter, highlighter, blog
category : life
tags : [life]
---

��Ϊһ����ʱ������ũ��д���Ͷ����Ǽ����Ե�blog�������ٲ���д���룬���û�и���֧�ֵĴ�����ʾ�����д����blog�Ǻܲ�ˬ��һ���¡����ԣ����쿪ʼΪ�ҵĲ���Ѱ��֧�ִ�������Ĳ����
Googleһ�£��ҵ���[��9�����õ�Javascript��������ű���](http://www.qianduan.net/9-useful-javascript-syntax-highlighting-scripts.html)��ƪ���£�ѡ���˵�һ�����[SyntaxHighlighter](http://alexgorbatchev.com/SyntaxHighlighter).

1. [����](http://alexgorbatchev.com/SyntaxHighlighter/download/)SyntaxHighlighter���������ص������µ�3.0.83�汾��
2. ������������
    ���shCore.js��shCore.css��shThemeDefault.css��Ҳ����ѡ��������theme.css�������page
	
			<script type="text/javascript" src="js/shCore.js"></script>
			<link href="css/shThemeDefault.css" rel="stylesheet" type="text/css" />
			<link href="css/shThemeDefault.css" rel="stylesheet" type="text/css" />
			
	�������Ҫ�����Ե�js������javascript����css/shBrushJScript.js
	
			<script type="text/javascript" src="css/shBrushJScript.js"></script>
	
	�������js���룺
	
			<script type="text/javascript">
				 SyntaxHighlighter.all()
			</script>

	��ɣ�
			
3. ��ҳ�������ʹ�ã������ַ�����

	��һ�֣�
    
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
	�ڶ��֣�PS��һ��Ҫ����CDATA����

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
	
	PS:Ϊ��������ʾԭ�棬�Ұ�< pre>��< script>��д���ˡ�����
    
    ���յ�Ч��Ϊ��
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
�����������Ƿ����SyntaxHighlighter���ĵ���ˮ��ˮ��~~