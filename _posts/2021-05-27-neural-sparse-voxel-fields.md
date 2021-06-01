---
title: 'Neural Sparse Voxel Fields'
date: 2014-08-14
permalink: /posts/2021/05/neural-sparse-voxel-fields/
tags:
  - voxel fields
  - scene representation
  - category2
---

由于场景理解任务需要捕捉到物体表面细节并且获得对应几何模型，故对真实场景进行自由视点的渲染非常具有挑战性。

3D的表示方式
====

- 点云
  
  点云是在3D空间中的集合，每个点都是一个坐标(xyz)表示，同时会具有其它特征(比如RGB颜色). 它们都是捕捉到的LIDAR数据的原始形式, 通常在进行下一步处理之前, 立体和RGB-D数据会转换成点云格式.
  
- Voxel网格

  Voxel网格是由点云演化而来. Voxel就像3D中的像素, 我们可以将Voxel想象成量化的、固定尺寸的点云. 尽管点云可以在空间中任何位置具有无限的点与浮点像素坐标,但是Voxel网格是3D网格, 其中每一个体素(Voxel)都有固定的尺寸和独立的坐标.

- 多边形网格

  多边形网格是一组具有共同顶点的多边形表面组成的一个近似几何形状的表面. 将点云想象成从连续几何表面采集的3D的点的集合, 多边形网格的目的是用一种易于渲染的方法表示出这些表面. 虽然最初是为了计算机图形而建立的, 多边形网格也可以用于3D视觉. 从点云中获取多边形网格的方法有很多, 可参考[Poisson Surface Reconstruction](http://hhoppe.com/poissonrecon.pdf).

- 多角度表示
  
  多角度表示是从多个角度捕捉到的、经过渲染的多边形网格的2D图像的集合.仅从多个相机中捕捉不同图像和创建多角度表示之间的区别在于, 多角度需要搭建一个完整的3D模型, 并且从多个任意角度进行渲染, 以完全传递潜在的几何图像. 与上面三种表示方式不同的是, 多角度表示通常将3D数据转换成更简单的形式以便用于处理数据可视化.

辐射场
====
周晓巍：“你可以把辐射场比作3D空间内各个点所发出光线的集合，记录了每个点的光线、颜色和密度，基于辐射场就可以渲染出各个视角的图像。”

## NeRF (Neural Radiance Field) 神经辐射场

神经辐射场NeRF是ECCV2020的paper, 其核心点是非显式地将一个复杂的静态场景用一个神经网络来模拟, 在网络训练完成后可以从任意角度渲染出清晰的场景图. 

### 算法概览

<img src="https://pic3.zhimg.com/80/v2-aaa7fba324ee5c3bba18a2b5fea3a616_720w.jpg" width = 70% height = 70% div align=center />

NeRF可以简要概括为用一个MLP神经网络去隐式的学习一个静态3D场景. 为了训练网络, 针对任意一个场景, 需要提供大量相机参数已知的图片. 基于这些图片训练好神经网络后, 就可以从任意角度渲染出图片结果了.

我们需要从如下三方面理解NeRF:
- 如何用NeRF来表示3D场景?
- 如何基于NeRF渲染2D图片?
- 如何训练NeRF?
即我们要理解NeRF是如何从一系列2D图像中学习到3D场景的, 又是如何渲染出2D图像的.

$$ \Vert\vec{x}\Vert_1=\sum_{i=1}^N\vert{x_i}\vert $$

Headings are cool
======

You can have many headings
====

Aren't headings cool?
------

Reference:
- [【NeRF论文笔记】用于视图合成的神经辐射场技术](https://zhuanlan.zhihu.com/p/360365941)