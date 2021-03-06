---
title: AutoML之超参数优化
tags:
  - AutoML
  - GridSearch
mathjax: true
date: 2019-02-24 00:05:22
categories:
top:
---
# 前言
AutoML是指尽量不通过人来设定超参数，而是使用某种学习机制，来调节这些超参数。这些学习机制包括传统的贝叶斯优化，多臂老虎机（multi-armed bandit），进化算法，还有比较新的强化学习。

我将AutoML分为**传统AutoML** ，自动调节传统的机器学习算法的参数，比如随机森林，我们来调节它的max_depth, num_trees, criterion等参数。 还有一类AutoML，则专注深度学习。这类AutoML，不妨称之为**深度AutoML** ，与传统AutoML的差别是，现阶段深度AutoML，会将神经网络的超参数分为两类，一类是与训练有关的超参数，比如learning rate, regularization, momentum等；还有一类超参数，则可以总结为网络结构。对网络结构的超参数自动调节，也叫 **Neural architecture search (nas)** 。而针对训练的超参数，也是传统AutoML的自动调节，叫 **Hyperparameter optimization (ho)**。

<!-- more -->

 我们已经看到机器学习在许多领域取得了成功应用，但是通常这些模型对正确选取超参数都有强烈依赖。
每一个机器学习系统都有超参数，自动机器学习(AutoML)最基本的任务就是自动选取超参数。深度神经网络的性能更是尤其依赖于网络的结构、正则项、最优化目标等相关超参数的选取。自动超参数优化(HPO)的重要性体现在：
 1. 它能显著减少机器学习应用耗费的人力；
 2. 提升机器学习算法的性能；
 3. 自动超参数优化提升了科学研究的可复用性，使科研之间的竞争更公平。因为HPO的可复现方式比手工搜索要清晰，而不同算法性能差异不会再因为超参数设置的影响。

HPO在实践中也遇到了重重困难：
 1. 对于大型模型、复杂的机器学习流程、大型数据集，进行模型评估是昂贵的；
 2. 配置空间是复杂并且高维的。况且通常你并不清楚一个算法的那些超参数需要优化，又是什么优化范围。
 3. 没办法获取损失函数中超参数的梯度；
 4. 当训练集大小有限时优化的参数组合又没有很强的泛化能力

# 网格搜索GridSearch：
 - 优点：
  1. 可以找到最好的参数

 - 缺陷：
  1. 随着超参数规模越来越大，GridSearch时间复杂度会呈指数型增长；
  2. 有一些参数比较重要一些参数不太重要，GridSearch就会浪费很多的资源在非重要的参数上

# 随机搜索RandomSearch
每个超参数独立sample一部分对应的参数，在随机的sample上搜索
 - 优点：
  1. 不会受限于超参数规模，不会有指数爆炸、组合爆炸的问题；
  2. 由于在不同的超参数之间是独立进行搜索，不会存在重要参数和非重要参数浪费资源的问题
  3. 在较少的迭代次数内比网格搜索找到更好的结果

 - 缺点：
  1. 不能保证找到最优参数

![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/2019-02-24-003234.png)

# 贝叶斯优化

## 问题定义
假设$A$表示一个具有$N$个超参数的机器学习算法，第$n$个超参数用`$\Lambda _{n}$`来表示，`$\mathbf { \Lambda } = \Lambda _ { 1 } \times \Lambda _ { 2 } \times \ldots \Lambda _ { N }$`表示*整个超参数配置空间*， `$\boldsymbol { \lambda } \in \mathbf { \Lambda }$`表示一个超参数向量，而`$\Lambda _{\lambda}$`表示`$\Lambda$`对`$\lambda$`的一个实例。

一个超参数的域可以是实数(比如learning rate)、整数(比如网络层数)、二元型(比如是否要用early stopping)、类别型(哪种优化目标)。

另外配置空间还具有*条件性*，例如一个超参数可能仅仅与另外一个超参数取某一特定值有关。条件参数空间可看成一个有项无环图。
给定数据集

Reference：

 1. [Chapter 1: Hyperparameter Optimization](https://www.automl.org/wp-content/uploads/2018/11/hpo.pdf)
 2. [AUTOML: METHODS, SYSTEMS, CHALLENGES (NEW BOOK)](https://www.automl.org/book/)
 3. [Optimizing the hyperparameter of which hyperparameter optimizer to use](https://roamanalytics.com/2016/09/15/optimizing-the-hyperparameter-of-which-hyperparameter-optimizer-to-use/)
