---
title: FaceNet论文笔记
date: 2017-06-28 10:11:42
categories: Face
tags: [笔记，人脸识别]
description:
mathjax:
---

*原文链接*:[FaceNet:A unified embedding for face recognition and clustering](https://arxiv.org/abs/1503.03832)

## 简介

FaceNet，可以直接将人脸图像映射到欧几里得空间，空间距离的长度代表了人脸图像的相似性，只要该映射空间生成，人脸识别、验证和聚类等任务就可以轻松完成。FaceNet在LFW数据集上的准确率为99.63%，在YouTube Faces 数据集上准确率为95.12%。

## 前言

FaceNet采用的是通过卷积神经网络学习将图像映射到欧几里得空间，空间距离之间和图片相似度相关：同一个人的不同图像的空间距离很小，不同人的图像的空间距离较大，只要该映射确定下来，相关的人脸识别任务就变得简单。

当前存在的基于深度神经网络的人脸识别模型使用了分类层：中间层为人脸图像的向量映射，然后以分类层作为输出层，这类方法的缺点就是不直接和效率不高。与当前方法不同，FaceNet直接使用基于triplets的LMNN（最大边界近邻分类）的loss函数训练神经网络，网络直接输出为128维度的向量空间。

just a test hahaha