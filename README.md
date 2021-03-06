# 目标检测学习总结

作者：@[李家丞](https://ddlee.cn)，[格灵深瞳](http://www.deepglint.com/)算法组实习生

审稿：张德兵，孙志成

## Links

首发于格灵深瞳知乎机构号：[干货 | 目标检测入门，看这篇就够了](https://zhuanlan.zhihu.com/p/34142321)

在线阅读及评论：[GitBook](https://www.gitbook.com/book/ddlee96/object-detection-review/details)

更新发布地址：[GitHub](https://github.com/ddlee96/Obj_Det_Review/releases)

Issue提交：[GitHub](https://github.com/ddlee96/Obj_Det_Review/issues/new)

## Changelog

### PDF v0.2 2018.03.16

- 调整部分图片大小
- 添加链接

### PDF v0.1 2018.03.12

- 全部内容完成
- 生成PDF文档

### Draft v0228

- 完成了拾遗部分工作的介绍
- 少量图片大小调整、连接的补充

## 引言

近年来，深度学习模型逐渐取代传统机器视觉方法而成为目标检测领域的主流算法，本系列文章将回顾早期的经典工作，并对较新的趋势做一个全景式的介绍，帮助读者对这一领域建立基本的认识。由于作者学历尚浅，水平有限，不实和不当之处也请指出和纠正，欢迎大家评论交流。

# 目录

- [（一）目标检测经典模型回顾](Chapter-1_Overview_and_classice_models/README.md)
- [（二）目标检测模型的评测与训练技巧](Chapter-2_Benchmarks_and_tricks/README.md)
- [（三）目标检测新趋势之基础网络结构演进、分类定位的权衡](Chapter-3_Backbone_networks_and_the_trade-off/README.md)
- [（四）目标检测新趋势之特征复用、实时性](Chapter-4_Feature_reuse_and_real_time_progress/README.md)
- [（五）目标检测新趋势拾遗](Chapter-5_Other_trends/README.md)

## 目标检测的任务表述

如何从图像中解析出可供计算机理解的信息，是机器视觉的中心问题。深度学习模型由于其强大的表示能力，加之数据量的积累和计算力的进步，成为机器视觉的热点研究方向。

那么，如何理解一张图片？根据后续任务的需要，有三个主要的层次。

![cv](static/img/cv.jpg) _图像理解的三个层次_

一是分类（Classification），即是将图像结构化为某一类别的信息，用事先确定好的类别(string)或实例ID来描述图片。这一任务是最简单、最基础的图像理解任务，也是深度学习模型最先取得突破和实现大规模应用的任务。其中，ImageNet是最权威的评测集，每年的ILSVRC催生了大量的优秀深度网络结构，为其他任务提供了基础。在应用领域，人脸、场景的识别等都可以归为分类任务。

二是检测（Detection）。分类任务关心整体，给出的是整张图片的内容描述，而检测则关注特定的物体目标，要求同时获得这一目标的类别信息和位置信息。相比分类，检测给出的是对图片前景和背景的理解，我们需要从背景中分离出感兴趣的目标，并确定这一目标的描述（类别和位置），因而，检测模型的输出是一个列表，列表的每一项使用一个数据组给出检出目标的类别和位置（常用矩形检测框的坐标表示）。

三是分割（Segmentation）。分割包括语义分割（semantic segmentation）和实例分割（instance segmentation），前者是对前背景分离的拓展，要求分离开具有不同语义的图像部分，而后者是检测任务的拓展，要求描述出目标的轮廓（相比检测框更为精细）。分割是对图像的像素级描述，它赋予每个像素类别（实例）意义，适用于理解要求较高的场景，如无人驾驶中对道路和非道路的分割。

本系列文章关注的领域是目标检测，即图像理解的中层次。
