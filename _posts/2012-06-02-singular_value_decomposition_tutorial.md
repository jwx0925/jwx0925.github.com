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

![2012-06-02-svd-1](/assets/images/2012-06-02-svd-1.jpg)

Let's look at the problem of trying to predict what score each player will make on a given hole. One idea is give each hole a HoleDifficulty factor, and each player a PlayerAbility factor. The actual score is predicted by multiplying these two factors together.

我们现在的问题是，试图预测每个运动员在每个洞的成绩。一种方案是，给每个洞一个难度系数（HoleDifficulty factor），同时给每个运动员一个能力系数（PlayerAbility factor），实际的得分就可以通过“难度系数 * 能力系数”来预测了。

	PredictedScore = HoleDifficulty * PlayerAbility

For the first attempt, let's make the HoleDifficulty be the par score for the hole, and let's make the player ability equal to 1. So on the first hole, which is par 4, we would expect a player of ability 1 to get a score of 4.

第一个尝试，我们把HoleDifficulty factor设定为每个洞的标准杆，平且设置每个运动员的PlayerAbility为1。这样，第一个洞的标准杆为4，所以预测的成绩为：1*4=4.

（一般情况下的高尔夫球场分为18个洞，每个洞的标准杆都不一样，但总杆数一般都是72杆。par的意思就是这一洞打完所用的杆数和标准杆持平。）

	PredictedScore = HoleDifficulty * PlayerAbility = 4 * 1 = 4
	
For our entire scorecard or matrix, all we have to do is multiply the PlayerAbility (assumed to be 1 for all players) by the HoleDifficulty (ranges from par 3 to par 5) and we can exactly predict all the scores in our example.

对于整个计分板矩阵，我们把PlayerAbility（假设都是1）乘以HoleDifficulty（标准杆范围：3-5），这样我们就能预测所有人的每个洞的分数了。

In fact, this is the one dimensional (1-D) SVD factorization of the scorecard. We can represent our scorecard or matrix as the product of two vectors, the HoleDifficulty vector and the PlayerAbility vector. To predict any score, simply multiply the appropriate HoleDifficulty factor by the appropriate PlayerAbility factor. Following normal vector multiplication rules, we can generate the matrix of scores by multiplying the HoleDifficulty vector by the PlayerAbility vector, according to the following equation.

事实上，这就是一个计分板矩阵的一维的奇异值分解。我们将我们的计分板矩阵表现2个向量（HoleDifficulty向量和PlayerAbility向量）的乘积。为了预测所有的得分，我们简单地将HoleDifficulty和PlayerAbility相乘。根据矩阵的乘法定律，我们得到一个得分矩阵。

![2012-06-02-svd-2](/assets/images/2012-06-02-svd-2.jpg)

Mathematicians like to keep everything orderly, so the convention is that all vectors should be scaled so they have length 1. For example, the PlayerAbility vector is modified so that the sum of the squares of its elements add to 1, instead of the current 12 + 12 + 12 = 3. To do this, we have to divide each element by the square root of 3, so that when we square it, it becomes 1/3 and the three elements add to 1. Similarly, we have to divide each HoleDifficulty element by the square root of 148. The square root of 3 times the square root of 148 is our scaling factor 21.07. The complete 1-D SVD factorization (to 2 decimal places) is:

数学家喜欢把一切都保持简单有序，所以我们的原则是缩放所有的向量，使其长度为1。例如，PlayerAbility向量所有元素的平方相加等于3：1^2+1^2+1^2=3。为了完成计算，我们必须把每个元素除以√3，这样得到每个元素为1/√3=0.58。同样，我们用同样的方法计算HoleDifficulty，比如第一个洞的难度系数为4/√148=0.33。这样缩放因子（scaling factor）为√148×√3=21.07

![2012-06-02-svd-3](/assets/images/2012-06-02-svd-3.jpg)

Our HoleDifficultyvector, that starts with 0.33, is called the Left Singular Vector. The ScaleFactor is the Singular Value, and our PlayerAbilityvector, that starts with 0.58 is the Right Singular Vector. If we represent these 3 parts exactly, and multiply them together, we get the exact original scores. This means our matrix is a rank 1 matrix, another way of saying it has a simple and predictable pattern.

HoleDifficulty向量叫左奇异向量，ScaleFactor叫做奇异值，PlayerAbility向量就是右奇异向量。如果我们把这3部分相乘，就会得到原始的分数。这意味着我们的矩阵是一个秩为1的矩阵，另一种方式说，它有一个简单的和可预测的模式。

##Extending the SVD with More Factors
##用更多的factor拓展SVD

More complicated matrices cannot be completely predicted just by using one set of factors as we have done. In that case, we have to introduce a second set of factors to refine our predictions. To do that, we subtract our predicted scores from the actual scores, getting the residual scores. Then we find a second set of HoleDifficulty2 and PlayerAbility2 numbers that best predict the residual scores.

用一个因子已经无法很好地预测很多复杂的矩阵。这样，我们不得不引入2个因子去提炼我们的预测结果。为此，我们用实际得分减去预测分数，得到“剩余的分数”。这样，我们可以算出HoleDifficulty2和PlayerAbility2向量，由此更好的预测“剩余分数”。

Rather than guessing HoleDifficulty and PlayerAbility factors and subtracting predicted scores, there exist powerful algorithms than can calculate SVD factorizations for you. Let's look at the actual scores from the first 9 holes of the 2007 Players Championship as played by Phil, Tiger, and Vijay.

相比于猜测HoleDifficulty和PlayerAbility因子，减去预测分数，还有更加强大的算法能够帮你算出SVD分解。让我们看看Phil, Tiger, and Vijay在2007年参加的9个洞的真实分数。

![2012-06-02-svd-4](/assets/images/2012-06-02-svd-4.jpg)

The 1-D SVD factorization of the scores is shown below. To make this example easier to understand, I have incorporated the ScaleFactor into the PlayerAbility and HoleDifficulty vectors so we can ignore the ScaleFactor for this example.

下面是一维的SVD分解。为了容易理解，我已经将ScaleFactor合并到PlayerAbility和HoleDifficulty向量中，所以我们可以暂时忽略ScaleFactor。

![2012-06-02-svd-5](/assets/images/2012-06-02-svd-5.jpg)

Notice that the HoleDifficulty factor is almost the average of that hole for the 3 players. For example hole 5, where everyone scored 4, does have a factor of 4.00. However hole 6, where the average score is also 4, has a factor of 4.05 instead of 4.00. Similarly, the PlayerAbility is almost the percentage of par that the player achieved, For example Tiger shot 39 with par being 36, and 39/36 = 1.08 which is almost his PlayerAbility factor (for these 9 holes) of 1.07.

我们注意到HoleDifficulty因子几乎是当前洞的3个运动员的平均成绩。比如，第5个洞，每个人都得4分，factor也是4。还有6号洞，平均得分也是4分，但是factor确实4.05。同样，PlayerAbility因子也几乎是运动员得分与标准杆的百分比，比如，Tiger一共得了39分，标准分是36，39/36 = 1.08，这个分数几乎和PlayerAbility factor的1.07相同。

Why don't the hole averages and par percentages exactly match the 1-D SVD factors? The answer is that SVD further refines those numbers in a cycle. For example, we can start by assuming HoleDifficultyis the hole average and then ask what PlayerAbility best matches the scores, given those HoleDifficulty numbers? Once we have that answer we can go back and ask what HoleDifficulty best matches the scores given those PlayerAbility numbers? We keep iterating this way until we converge to a set of factors that best predict the score. SVD shortcuts this process and immediately give us the factors that we would have converged to if we carried out the process.

但是，为什么杆数平均值（hole averages）和标准杆百分比（par percentages）不是完全等于一维的SVD呢？答案是，SVD进一步的提炼了这些数字。比如，我们开始先假设HoleDifficulty是杆数平均值（hole averages），然后同这个平均值去计算最匹配的PlayerAbility。一旦我们有了答案，我们可以返回去，问如果给出PlayerAbility，然后去计算最匹配的HoleDifficulty？我们不断的迭代，直到收敛到一组能够很好预测分数的factors。SVD缩短了这个过程，立即给我们这些收敛后的factors。

One very useful property of SVD is that it always finds the optimal set of factors that best predict the scores, according to the standard matrix similarity measure (the Frobenius norm). That is, if we use SVD to find the factors of a matrix, those are the best factors that can be found. This optimality property means that we don't have to wonder if a different set of numbers might predict scores better.

根据标准矩阵的相似度（Frobenius范数），SVD一个非常有用的功能就是找出能够最好的预测分数的factors。也就是说，如果用SVD寻找这些factors，通常这些factors是很好地。这个很好的功能意味着，我们不必怀疑是不是别的数字能够预测出更好的数据。

Now let's look at the difference between the actual scores and our 1-D approximation. A plus difference means that the actual score is higher than the predicted score, a minus difference means the actual score is lower than the prediction. For example, on the first hole Tiger got a 4 and the predicted score was 4.64 so we get 4 - 4.64 = -0.64. In other words, we must add -0.64 to our prediction to get the actual score.

现在，我们看一下实际分数和一维近似值的却别。正数表示实际分数比预测分数高，负数表示实际分数比预测分数低。比如，在一个洞，Tiger得了4分，预测分数为4.64，所以我们获得4 - 4.64 = -0.64。也就是说，预测值加上-0.64才能得到实际值。

Once these differences have been found, we can do the same thing again and predict these differences using the formula HoleDifficulty2 * PlayerAbility2. Since these factors are trying to predict the differences, they are the 2-D factors and we have put a 2 after their names (ex. HoleDifficulty2) to show they are the second set of factors.

一旦发现了这些不同，我们就可以不断的做同样的事，通过公式HoleDifficulty2 * PlayerAbility2来预测不同。因为这些factors试图预测这些不同，所以我们现在有2二维factors，我们将其命名为HoleDifficulty2来表明他们是第二组factors。

![2012-06-02-svd-6](/assets/images/2012-06-02-svd-6.jpg)

There are some interesting observations we can make about these factors. Notice that hole 8 has the most significant HoleDifficulty2 factor (-1.29). That means that it is the hardest hole to predict. Indeed, it was the only hole on which none of the 3 players made par. It was especially hard to predict because it was the most difficult hole relative to par (HoleDifficulty - par) = (3.39 - 3) = 0.39, and yet Phil birdied it making his score more than a stroke below his predicted score (he scored 2 versus his predicted score of 3.08). Other holes that were hard to predict were holes 3 (0.80) and 7 (0.89) because Vijay beat Phil on those holes even though, in general, Phil was playing better.

我们通过这些factors可以发现一些有趣的现象。注意第8号洞有一个最大波动的HoleDifficulty2值（-1.29）。这意味着，这个洞是很难预测的。实际上，3个运动员没有一个人在这个洞打了标准杆（3杆，3个运动员分别打了2\4\4杆）。这个洞是很难预测的，……即使Phil打了一杆小鸟球，使其分数低于预测分数（他得了2分，但是预测确实3.08分）。其他比较难预测的洞是3号洞（0.80）和7号洞（0.89），因为即使在这两个洞，Vijay都击败了Phil，但总体来看，Phil还是打得更好一些。

##The Full SVD Factorization

The full SVD for this example matrix (9 holes by 3 players) has 3 sets of factors. In general, a m x n matrix where m >= n can have at most n factors, so our 9 x 3 matrix cannot have more than 3 sets of factors. Here is the full SVD factorization (to two decimal places).

这个例子中（9个洞 * 3个运动员），一个完整的SVD是有3个factors。事实上，一个m*n的矩阵，在m>n的情况下最多有n个factors，所以我们的9*3的矩阵不能有大于3个factors。下面是完整的SVD分解结果：

![2012-06-02-svd-7](/assets/images/2012-06-02-svd-7.jpg)

By SVD convention, the HoleDifficulty and PlayerAbility vectors should all have length 1, so the conventional SVD factorization is:

按照SVD的约定，HoleDifficulty和PlayerAbility矩阵所有的长度都必须是1，所以我们完整的SVD分解结果为：

![2012-06-02-svd-8](/assets/images/2012-06-02-svd-8.jpg)

##Latent Semantic Analysis Tutorial

We hope that you have some idea of what SVD is and how it can be used. Next we'll cover how SVD is used in our Latent Semantic Analysis Tutorial. Although the domain is different, the concepts are the same. We are trying to predict patterns of how words occur in documents instead of trying to predict patterns of how players score on golf holes.
