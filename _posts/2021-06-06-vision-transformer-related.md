---
title: 'Vision Transformer Related Papers'
date: 2021-06-06
permalink: /posts/2021/06/vit-related/
tags:
  - vit
  - transformer
---

# Intriguing Properties of Vision Transformer

Vision transformer 有哪些有趣的特性？

谷歌研究员等发现了Transformer一些有趣的特性, 如对严重的遮挡, 扰动和域偏移等具有很高的鲁棒性, 与CNN相比, ViT更加符合人类直觉, 泛化性更强.

## 简介

近期, ViT在各个垂直任务上都取得了不错的成绩, 这些模型基于multi-head自注意力机制, 该机制可以灵活地处理一系列图像patches以对上下文cues进行编码.

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZLIVsvV9nZ8bmJD4aYjV89OEibW6DnFl0kcLeib2DQDOeH4es9ZalJSug/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=50% /></div>

一个重要的问题是， 在以给定patch为条件的图像范围内, 如何灵活地处理图像中的干扰, 例如严重的遮挡, 域偏移, 空间排列, 对抗性和自然扰动等等问题. 作者通过涵盖3个ViT系列的大量实验, 以及CNN网络结构进行比较, 系统性的研究了上述问题, 并得到了如下结果:

1. Transformer对严重的遮挡, 扰动, 和域偏移具有很高的鲁棒性, 例如, 即使随机遮挡80%的图像内容, 其在ImageNet上仍然可以保持60%的top-1精度;
2. Transformer对遮挡的良好表现并不是依赖于局部的纹理信息, 与CNN相比, **ViT对纹理的依赖小很多**. 当经过适当训练以对基于shape的特征进行编码时, ViT可以展现出与人类视觉相当的shape识别能力, 例如只从shape线索中识别素描和绘画图片;
3. ViTs对其它不利因素也具有较高的鲁棒性, 比如patch permutations, 对抗扰动(adversarial pertubations)和常见的扰动(噪声, 模糊, 对比度, artifacts等)均有着较高的鲁棒性.
4. ViTs在未见过的domain上也都表现十分良好, 比如few-shot learning, fine-grained recognition, scene classification和long-tail recognition setting.
5. 除此之外, ViT还有很多值得挖掘的潜力, 比如本文提出了一个改进版的DeiT用于编码形状信息, 具体地, 引入了一个dedicated token, 该token证明了如何在相同的网络结构中使用不同的token来对看似矛盾的线索进行建模, 从而产生例如无像素级监督的自动分割的有利影响.

## 本文讨论主题

### ViT对遮挡是否鲁棒?

假设有一个网络模型$f$, 它通过处理一个输入图像$x$来预测一个标签$y$, 其中, $x$可以表示为一个patch $x={x_i}_{i=1}^{N}$的序列, $N$是图像patch的总数.

虽然可以有很多方法建模遮挡, 本文还是选择了一个简单的遮挡策略, 选择整张图像patch的一个子集$M, M<N$, 并将这些patches像素设置为0, 这样便创建了一个遮挡图片$x^{\prime}$.

作者将上述方法称为PatchDrop, 目的是观察鲁棒性$f(x^{\prime})_{argmax}=y$.

作者一共总结了三种不同的遮挡方法:

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0Z27d2ta8EV9OtZicT5wxEPSww62icHQmAYEzIVczNynOiaZd9IQk7MIFvQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=70% /></div>

1. Random PatchDrop

    ViT通常将一幅$224\times 224 \times 3$的图像划分为$14\times 14=196$个patch, 每个patch的大小为$16\times 16\times 3$. 例如, 随机从输入中删除100个这样的patch, 就相当于失去了51%的图像内容.

2. Salient(foreground) PatchDrop

    对于分类器来说, 并不是所有的像素都同样重要, 为了估计显著区域, 作者利用一个自监督的ViT模型DINO, 该模型使用注意力分割图像中的显著目标. 按照这种方法, 作者可以删除一部分有前景信息的patch.

3. Non-Salient(background) PatchDrop

    方法同上, 但是删除具有背景信息的patch.

#### ***鲁棒性分析:***

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_png/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZKU8OFpCAXibu1ILlZyuR8Je4kXeo3jPsyIiauiaLaRqSo0u857eMNzFCQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=80%/></div>

以Random PatchDrop为例, 作者给出五次测试的平均值和标准差, 对于显著性和非显著性PatchDrop, 由于获得遮挡掩模是确定的, 故只给了运行一次的精度值.

从如上结果可以看出, Random PatchDrop 50%的图像使得CNN完全丧失了识别能力, 而DeiT等Transformer-based models均有着较高的识别精度, 更极端的, 当90%图像信息都丢失时, DeiT-B仍然有37%的识别能力. 同样的, 在显著性和非显著性PatchDrop的条件下, ViTs均有着不俗的表现.

#### ***Class Token Perserves Information:***

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0Z2PtibKqjlatGMBmF7aiad1IPClSrG2yEZj2XkOC75oP4AeUAWOjGFiciaA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=80% /></div>

为了更好的了解ViTs模型对遮挡具有鲁棒性的原因, 作者将DeiT-B的不同层的attn. map均进行了可视化, 如上图所示. 可以看出, ViTs的浅层更关注遮挡区域, 较深的层更加关注遮挡以外的信息.

为了探究从浅层到深层的token的变化是否导致了基于遮挡的token不变性, 作者计算了**原始图像**和**遮挡图像**的特征/标tokens的相关系数. 测试ResNet50的相关系数时, 作者使用了logit layer前的feats. 测试ViTs的相关系数时, class token从最后一个Transformer block中获得, 测试结果如下:

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZKUqhxiaU381GJsBpcZYXTOS4lklVo1ib2USYb8Bphia3eS30a1ZK9TNJg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=50%/></div>

此外, 作者还可视化了ImageNet中12个子类的相关系数, 可视化结果如下图. 可以看出, 对于不同的类来说, 即使类的目标很小, 比如昆虫, 食物, 鸟类等, 这种现象均存在.

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZjA7zlodvz7LcPhvkxJEzmo4BUp2krWdSAMaEU208rHAEJGDrk4nBNQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=50%/></div>

### ViTs是否可以同时学习Shape和Texture的相关信息?

Geirhos等人引入了Shape v.s. Texture的假设, 并提出了一种训练框架来增强卷积神经网络的Shape偏差. 类似地, 作者也对ViT模型进行了类似分析, 结果表明, ViTs可获得比CNNs更强的Shape偏差, 但是这种方法会导致对正常图像的精度下降.

为了解决这种问题, 作者另外将shape token引入ViTs结构中, 专门学习shape信息. 具体来说, 作者使用一组不同的tokens在同一ViTs网络结构中分别对Shape和Texture进行建模. 为此, 作者从预训练的强shape-bias的CNN模型中蒸馏shape相关信息, 这种蒸馏方法既保证了合理的分类精度, 也提供了比原始ViTs模型更好的shape偏差.



Reference:
- [谷歌研究员：Transformer那些有趣的特性](https://mp.weixin.qq.com/s?src=11&timestamp=1622772800&ver=3109&signature=xxtu8aeppESTqpe1jCLbPKBtNoKIBW0gV0PZw7eT2RyoFjzbur0OJBg8UmKrOtWRgW3Ab7rG7UnGgK4WXsjI7AeJAp0lYfyuqpLZbeptuym80J-KKqveQnkPYveVuqcX&new=1)