---
title: 朴素贝叶斯
date: 2018-2-17
tags: [Bayes, Python, MachineLearn]
mathjax: true
---

朴素贝叶斯算法是监督学习算法的一种，属于一种线性分类器，理解朴素贝叶斯就要先说贝叶斯定理。

#### 贝叶斯定理

**条件概率**：常常要计算在某个事件B发生的条件下，另一个事件A发生的概率。概率论中称此概率为事件B已发生的条件下事件A发生的条件概率，记为P(A|B)。
<!-- more -->
$P(A|B) = \frac{P(A\cap B)}{P(B)}$
同理可得：
$P(A\cap B) = P(A|B)P(B)$

$P(A\cap B) = P(B|A)P(A)$

即:
$P(A|B)P(B) = P(B|A)P(A)$

**全概率公式**：假定样本空间S，是两个事件A与A'的和。



![img](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011082503.jpg)

图中，红色部分是事件A，绿色部分是事件A'，它们共同构成了样本空间S。

在这种情况下，事件B可以划分成两个部分。

![img](http://www.ruanyifeng.com/blogimg/asset/201108/bg2011082504.jpg)

可以写出B的概率：
$P(B) = P(B\cap A) + P(B\cap A')$
由上条件概率可得：
$P(B) = P(B|A)P(A) + P(B|A')P(A')$
这就是全概公式。

到这里贝叶斯定理也就呼之欲出了,


$P(A|B) =  \frac{P(B|A)P(A)}{P(B|A)P(A)+P(B|A')P(A')}$

#### 朴素贝叶斯分类器

朴素贝叶斯分类器是一种基于贝叶斯定理的弱分类器，所有朴素贝叶斯分类器都假定样本每个特征与其他特征都不相关。

也就是为什么叫 Naive Bayes.

举个例子，如果一种水果其具有红，圆，直径大概3英寸等特征，该水果可以被判定为是苹果。尽管这些特征相互依赖或者有些特征由其他特征决定，然而朴素贝叶斯分类器认为这些属性在判定该水果是否为苹果的概率分布上独立的。

条件概率可以变形得到：
$P(A|B) = \frac {P(A\cap B)}{P(B)} = P(A)\frac {P(B|A)}{P(B)}$
其中P(A)称为"先验概率"，即在B事件发生之前，我们对A事件概率的一个判断。P(A|B)称为"后验概率"，即在B事件发生之后，我们对A事件概率的重新评估。P(B|A)/P(B)称为"可能性函数"，这是一个调整因子，使得预估概率更接近真实概率。

```python
后验概率　＝　先验概率 ｘ 调整因子
```

#### 朴素贝叶斯分类流程

由一个天气和响应目标变量“玩”的训练数据集（计算“玩”的可能性）。我们需要根据天气条件进行分类，判断这个人能不能出去玩，以下是步骤：

步骤1：将数据集转换成频率表；

步骤2：计算不同天气出去玩的概率，并创建似然表，如阴天的概率是0.29；

![img](http://mmbiz.qpic.cn/mmbiz_png/hq0PKaHicMTFortVtEPN92OlBXYWPy1lWpYa9rZuDh8ia4Wyf1l7WyVIicogySAIPUmRRAXZBbZLVpyQib244yOibKg/?tp=webp&wxfrom=5&wx_lazy=1)

步骤3：使用贝叶斯公式计算每一类的后验概率，数据最高那栏就是预测的结果。

问题：如果是晴天，这个人就能出去玩。这个说法是不是正确的？

P(是|晴朗)=P(晴朗|是)×P(是)/P(晴朗)

在这里，P(晴朗|是)= 3/9 = 0.33，P(晴朗)= 5/14 = 0.36，P(是)= 9/14 = 0.64

现在，P(是|晴朗)=0.33×0.64/0.36=0.60，具有较高的概率。

#### Python 实现朴素贝叶斯分类器

scikit learn里有3种朴素贝叶斯的模型：

**高斯模型**：适用于多个类型变量，假设特征符合高斯分布。

**多项式模型**：用于离散计数。如一个句子中某个词语重复出现，我们视它们每个都是独立的，所以统计多次，概率指数上出现了次方。

**伯努利模型**：如果特征向量是二进制（即0和1），那这个模型是非常有用的。不同于多项式，伯努利把出现多次的词语视为只出现一次，更加简单方便。

以高斯模型为例：

```python
from sklearn.naive_bayes import GaussianNB
import numpy as np
x= np.array([[-3,7],[1,5], [1,2], [-2,0], [2,3], [-4,0], [-1,1], [1,1], [-2,2], [2,7], [-4,1], [-2,7]])
Y = np.array([3, 3, 3, 3, 4, 3, 3, 4, 3, 4, 4, 4])
# 建立分类器
clf = GaussianNB()
# 拟合数据
clf.fit(x, y)
# 分类
predicted= model.predict([[1,2],[3,4]])
```



#### 参考文章

[贝叶斯推断及其互联网应用（一）：定理简介](http://www.ruanyifeng.com/blog/2011/08/bayesian_inference_part_one.html)

[6步骤带你了解朴素贝叶斯分类器](https://mp.weixin.qq.com/s/UXvSe_FYcS_s5HicMV6SUA)
