---
layout: post
title: 奇异值分解简明教程（翻译）
description: UML、聚合关系和组合关系、关联关系和依赖关系
keywords: svd,算法
category: 语义分析
tags: [语义分析]
---

##Singular Value Decomposition (SVD) Tutorial
##奇异值分解简明教程

When you browse standard web sources like Singular Value Decomposition (SVD) on Wikipedia, you find many equations, but not an intuitive explanation of what it is or how it works. Singular Value Decomposition is a way of factoring matrices into a series of linear approximations that expose the underlying structure of the matrix.
当你访问一些解释“奇异值分解”的文章时，这些文章通常会有很多方程式，而不是直观的解释SVD的分解原理。奇异值分解是一种将一个矩阵分解成一系列线性相关的数组，以展示出这个数组的潜在特性。

SVD is extraordinarily useful and has many applications such as data analysis, signal processing, pattern recognition, image compression, weather prediction, and Latent Semantic Analysis or LSA (also referred to as Latent Semantic Indexing or LSI).
SVD非常有实际应用价值，例如数据分析，信号处理，模式识别，图像压缩，天气预测和潜在语义分析（也可以叫潜在语义索引LSI）


##How does SVD work?
##奇异值分解如何工作？

As a simple example, let's look at golf scores. Suppose Phil, Tiger, and Vijay play together for 9 holes and they each make par on every hole. Their scorecard, which can also be viewed as a (hole x player) matrix might look like this.
举个简单的例子：高尔夫比赛中的比分。假设Phil, Tiger, and Vijay在一起比赛，一共9个洞。下面的记分牌可以理解为一个矩阵（洞 * 运动员）。
Hole	Par	Phil	Tiger	Vijay
1	4	4	4	4
2	5	5	5	5
3	3	3	3	3
4	4	4	4	4
5	4	4	4	4
6	4	4	4	4
7	4	4	4	4
8	3	3	3	3
9	5	5	5	5
Let's look at the problem of trying to predict what score each player will make on a given hole. One idea is give each hole a HoleDifficulty factor, and each player a PlayerAbility factor. The actual score is predicted by multiplying these two factors together.
我们现在的问题是，试图预测每个运动员在每个洞的成绩。一种方案是，给每个洞一个难度系数（HoleDifficulty factor），同时给每个运动员一个能力系数（PlayerAbility factor），实际的得分就可以通过“难度系数 * 能力系数”来预测了。

	PredictedScore = HoleDifficulty * PlayerAbility

For the first attempt, let's make the HoleDifficulty be the par score for the hole, and let's make the player ability equal to 1. So on the first hole, which is par 4, we would expect a player of ability 1 to get a score of 4.
第一个尝试，我们把HoleDifficulty factor设定为每个洞的标准杆，平且设置每个运动员的PlayerAbility为1。这样，第一个洞的标准杆为4，所以预测的成绩为：1*4=4.

