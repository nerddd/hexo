---
title: Triplet loss
date: 2017-06-02 20:26:35
categories: 论文笔记
tags: [深度学习，人脸识别]
description: Triplet loss论文阅读笔记
mathjax: true
---

# 原理

Triplet是一个三元组，这个三元组是这样构成的：从训练数据集中随机选一个样本，该样本称为Anchor，然后再随机选取一个和Anchor (记为x_a)属于同一类的样本Positive (记为x_p)和不同类的样本Negative (记为x_n)，由此构成一个（Anchor，Positive，Negative）三元组。





![Triplet Loss 示意图](http://img.blog.csdn.net/20160727090101355)



### Triplet loss中的margin取值分析

我们的目的是为了让loss在训练迭代中下降的越小越好，即使Anchor和Positive越接近越好，Anchor和Negative越远越好，并且要让x_a与x_n之间的距离和x_a与x_p之间的距离之间有一个最小的间隔。简而言之，Triplet loss就是要使类内距离越小，类间距离越大。

```
当 margin 值越小时，loss 也就较容易的趋近于 0，于是 Anchor 与 Positive 都不需要拉的太近，Anchor 与 Negative 不需要拉的太远，就能使得 loss 很快的趋近于 0。这样训练得到的结果，不能够很好的区分相似的图像。

当 Anchor 越大时，就需要使得网络参数要拼命地拉近 Anchor、Positive 之间的距离，拉远 Anchor、Negative 之间的距离。如果 margin 值设置的太大，很可能最后 loss 保持一个较大的值，难以趋近于 0 。

因此，设置一个合理的 margin 值很关键，这是衡量相似度的重要指标。简而言之，margin 值设置的越小，loss 很容易趋近于 0 ，但很难区分相似的图像。margin 值设置的越大，loss 值较难趋近于 0，甚至导致网络不收敛，但可以较有把握的区分较为相似的图像。
```



## 相关

区分相似图形，除了triplet loss，还有一篇CVPR：[《Deep Relative Distance Learning: Tell the Difference Between Similar Vehicles》](http://blog.csdn.net/u010167269/article/details/51783446)提出的Coupled Cluster Loss.



**本文参考**：

[triplet loss 原理以及梯度推导](http://www.voidcn.com/blog/tangwei2014/article/p-4415770.html)

[如何在Caffe中增加layer以及Caffe中triplet loss layer的实现](http://www.voidcn.com/blog/mao_kun/article/p-6246924.html)









