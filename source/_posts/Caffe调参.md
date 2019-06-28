---
title: Caffe调参
date: 2017-06-17 09:26:12
categories: 深度学习
tags: [caffe,调参]
description: caffe调参的一些经验
mathjax:
---

# loss为nan

## 梯度爆炸

**原因**：梯度变得非常大，使得学习过程难以继续
**现象：**观察log，注意每一轮迭代后的loss。loss随着每轮迭代越来越大，最终超过了浮点型表示的范围，就变成了NaN。
**措施**： 
1. 减小solver.prototxt中的base_lr，至少减小一个数量级。如果有多个loss layer，需要找出哪个损失层导致了梯度爆炸，并在train_val.prototxt中减小该层的loss_weight，而非是减小通用的base_lr。 
2. 设置clip gradient，用于限制过大的diff

## 不当的损失函数
**原因**：有时候损失层中loss的计算可能导致NaN的出现。比如，给InfogainLoss层（信息熵损失）输入没有归一化的值，使用带有bug的自定义损失层等等。
**现象**：观测训练产生的log时一开始并不能看到异常，loss也在逐步的降低，但突然之间NaN就出现了。
**措施**：看看你是否能重现这个错误，在loss layer中加入一些输出以进行调试。
示例：有一次我使用的loss归一化了batch中label错误的次数。如果某个label从未在batch中出现过，loss就会变成NaN。在这种情况下，可以用足够大的batch来尽量避免这个错误。

## 不当的输入
**原因**：输入中就含有NaN。
**现象**：每当学习的过程中碰到这个错误的输入，就会变成NaN。观察log的时候也许不能察觉任何异常，loss逐步的降低，但突然间就变成NaN了。
**措施**：重整你的数据集，确保训练集和验证集里面没有损坏的图片。调试中你可以使用一个简单的网络来读取输入层，有一个缺省的loss，并过一遍所有输入，如果其中有错误的输入，这个缺省的层也会产生NaN。
**案例**：有一次公司需要训练一个模型，把标注好的图片放在了七牛上，拉下来的时候发生了dns劫持，有一张图片被换成了淘宝的购物二维码，且这个二维码格式与原图的格式不符合，因此成为了一张“损坏”图片。每次训练遇到这个图片的时候就会产生NaN。良好的习惯是，你有一个检测性的网络，每次训练目标网络之前把所有的样本在这个检测性的网络里面过一遍，去掉非法值。

**池化层中步长比核的尺寸大**
如下例所示，当池化层中stride > kernel的时候会在y中产生NaN
```
    layer {
      name: "faulty_pooling"
      type: "Pooling"
      bottom: "x"
      top: "y"
      pooling_param {
      pool: AVE
      stride: 5
      kernel: 3
      }
    }
```
**致谢**
*http://stackoverflow.com/questions/33962226/common-causes-of-NaNs-during-training*

# Accuracy一直为0

考虑标签是否从0开始递增

