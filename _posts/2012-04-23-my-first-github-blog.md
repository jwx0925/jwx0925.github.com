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

<pre class="brush: java">
package try_test;

public class TestTry {

    public static void main(String[] args) {
        /**
            inner try 
            outter try 
            return success!
         */
        System.out.println(doTry());

    }

    /**
     * @return 
     * 
     */
    private static String doTry() {
        try {
            System.out.println("inner try ");
            return "return success!";
        } finally {
            System.out.println("outter try ");
        }
    }

}
</pre>