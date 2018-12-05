---
title: Center loss笔记
date: 2017-06-29 17:05:51
categories: 论文笔记
tags: [深度学习，论文笔记]
description: Center loss论文阅读笔记
mathjax:
---

论文：[A Discriminative Feature Learning Approach for Deep Face Recognition](https://link.springer.com/chapter/10.1007%2F978-3-319-46478-7_31)

# 摘要

对于一般的CNN网络，softmax通常作为监督信号来训练深层网络，为了增强提取的特征的可辨别性（discriminative），提出center loss，应对人脸识别任务。center loss同时学习每个类别的深层特征中心和惩罚深层特征和它们对应的类中心的距离（the center loss simultaneously learns a center for deep
features of each class and penalizes the distances between the deep features and their corresponding class centers）。将softmax和center loss联合起来，可以训练一个健壮的CNN来获取深层特征的两个关键目标，类内紧凑和类间分散。

# 介绍

预先收集所有可能的测试身份用于训练是不现实的，所以CNN的标签预测并不总是适用的。经过深层网络获取得到的特征不仅需要可分开性(separable)，更需要识别性(discriminative)和广义性，足够用来识别新的没有遇见过的类。可识别性的特征可以利用最邻近(NN)或k-NN算法很好的分类，就没有必要依赖标签预测了。但是softmax只能产生可分开(separable)特征，结果特征就不足以用以人脸识别。

![Separabale Feature Vs Discriminative Feature](https://static.leiphone.com/uploads/new/article/740_740/201612/585bb8742235c.png?imageMogr2/format/jpg/quality/90)



因为随机梯度下降(SGD)优化CNN是基于mini-batch，不能很好的反映深层特征的全局分布，由于训练集的庞大，在每次迭代中输入所有的训练样本是不现实的。constractive loss和triplet loss分别作为图像对和三元组的loss函数。然而，与图像样本相比，训练图像对或三元组的数量显著增长，导致收敛缓慢和不稳定性。仔细选择图像对或者三元组，问题可能会部分缓解，但是它增加了计算复杂度，训练过程变得不方便。为了解决这个问题，提出center loss，用于有效的增强特征的可识别性，我们将会得到每个类的深层特征的中心。在训练阶段，我们同时更新中心和最小化特征和它们相应的类中心的距离。CNN同时在softmax loss和center loss的监督下进行训练，通过一个超参来平衡这两个监督信号。直觉上，softmax loss将不同类别的特征分开，center loss有效的将同一类别的特征拉向类的中心，使得类内特征分布变得紧凑。













