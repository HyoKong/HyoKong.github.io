---
title: 'paper-notes'
date: 2016-06-01
permalink: /posts/2021/06/paper-notes/
tags:
  - paper notes
---

# Stereo

## ***Boosting Monocular Depth Estimation Models to High-Resolution via Content-Adaptive Multi-Resolution Merging***

- CVPR 2021
- Citation 0
- Simon Fraser University, Adobe Research

<img src="http://yaksoy.github.io/images/hrdepthTeaser.jpg" width = 50% height = 50% div align=center />

### Poster
<img src='http://yaksoy.github.io/highresdepth/CVPR21PosterSm.jpg' width = 50% div align=mid>

### Intro

Issues:
- Trade-off between **a consistent scene structure** and **the high-frequency details**.
- The networks starts to produce **structurally inconsistent** when the **contextural cues** in the image are **further apart** than the **receptive field size**.

Methods:
- Using an edge map as the proxy for contextual cues to derermine this maximun resolution by making sure that no pixel is further apart from contextual cues than half of the receptive field size.


# 3D Hands and Pose

Reference:
- [【NeRF论文笔记】用于视图合成的神经辐射场技术](https://zhuanlan.zhihu.com/p/360365941)