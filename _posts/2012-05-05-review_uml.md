﻿---
layout: post
title: Uml中各种关系复习
description: UML、聚合关系和组合关系、关联关系和依赖关系
keywords: uml,dependency,association,aggregation，composition
category: uml
tags: [uml]
---

##UML回想ing
这几天工作中需要写系统分析说明书，对于系统中关键的领域模型进行分析，并画出uml类图。由于很久不看了，uml都快忘了怎么画了，所以先Google下，在网上找到了这个经典的图：

![2012-05-05-pattern_uml](/assets/images/2012-05-05-pattern_uml.PNG)

图中用例子清晰地解释了各种关系的含义了。简单解释为：
1. *关联*：连接模型元素及链接实例，用一条实线来表示；
2. *依赖*：表示一个元素以某种方式依赖于另一个元素，用一条虚线加箭头来表示；
3. *聚合*：表示整体与部分的关系，用一条实线加空心菱形来表示；
4. *组合*：表示整体与部分的有一关系，用一条实线加实心菱形来表示；
5. *泛化（继承）*：表示一般与特殊的关系，用一条实线加空心箭头来表示；
6. *实现*：表示类与接口的关系，用一条虚线加空心箭头来表示；


![2012-05-05-pattern_uml_explain](/assets/images/2012-05-05-pattern_uml_explain.jpg)


##聚合关系和组合关系
自我感觉上述中的聚合和组合模式讲得还不是很清晰，我们继续分析。
其实上面那个图片中的例子解析的比较清晰：

1. 鸟和翅膀是组合关系。没有鸟，翅膀不能单独存在，两者之间的关系比较强。
2. 雁群和大雁是组合关系。没有雁群，大雁照样可以单独存在，两者之间的关系没那么强。

这就是为什么组合是实心菱形（比较结实），聚合是空心菱形（关系较弱）了。哈哈~

另外，在[http://www.cnblogs.com/yunsean/archive/2010/11/01/1866206.html](http://www.cnblogs.com/yunsean/archive/2010/11/01/1866206.html)摘录了一段话，对于聚合和组合的解析也很经典：

>聚合和组合的区别在于：聚合关系是“*has-a*”关系，组合关系是“*contains-a*”关系；聚合关系表示整体与部分的关系比较弱，而组合比较强；聚合关系中代表部分事物的对象与代表聚合事物的对象的生存期无关，一旦删除了聚合对象不一定就删除了代表部分事物的对象。组合中一旦删除了组合对象，同时也就删除了代表部分事物的对象。
>我们用浅显的例子来说明聚合和组合的区别。“国破家亡”，国灭了，家自然也没有了，“国”和“家”显然也是组合关系。而相反的，计算机和它的外设之间就是聚合关系，因为它们之间的关系相对松散，计算机没了，外设还可以独立存在，还可以接在别的计算机上。在聚合关系中，部分可以独立于聚合而存在，部分的所有权也可以由几个聚合来共享，比如打印机就可以在办公室内被广大同事共用。


在《Java与模式》中解释组合关系时写到：代表整体的对象负责代表部分的对象的生命周期，且不能由多个“整体”共享一个“部分”。这本书中也举了一个例子：美猴王和他的四肢是组合关系，因为他的四肢由他自己复杂，不能共享；而美猴王和金箍棒之前是聚合关系，因为金箍棒也可以让深海龙王拿着，没了金箍棒美猴王照样是美猴王！

在*代码实现上*，关联关系，聚合关系和组合关系都是一样：*部分*在*整体*内部的类变量。


##关联关系和依赖关系
这两个关系在画图上有点像：关联关系是实线箭头；依赖关系是虚线箭头（见上图）。但是含义区别还是很大的。

*关联关系*比较普通，比聚合关系和组合关系更加通俗一点，就是两者有关系。这种关系比较多样化，所以关联关系可以是双向的，也可以是单向的。双向的关联可以有两个箭头或者没有箭头，单向的关联有一个箭头。实现方面，关联关系是通过使用成员变量来实现的。
<pre class="brush: java">
	class TuDi {
	};

	class TangSeng {
		protected list<TuDi> tdlist;
	};
</pre>
![2012-05-05-uml-association](/assets/images/2012-05-05-uml-association.jpg)

*依赖关系*也是类与类之间的联结，但是依赖总是单向的。依赖关系在 Java 或 C++ 语言中体现为局部变量、方法的参数或者对静态方法的调用。
<pre class="brush: java">
public class Person {
     public void buy(Car car) 
     {
         //...
     }
}
</pre>
![2012-05-05-uml-dependency](/assets/images/2012-05-05-uml-dependency.jpg)