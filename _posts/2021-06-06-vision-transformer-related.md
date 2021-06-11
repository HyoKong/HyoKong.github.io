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

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZLIVsvV9nZ8bmJD4aYjV89OEibW6DnFl0kcLeib2DQDOeH4es9ZalJSug/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=50% ></div>

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

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0Z27d2ta8EV9OtZicT5wxEPSww62icHQmAYEzIVczNynOiaZd9IQk7MIFvQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=70% ></div>

1. Random PatchDrop

    ViT通常将一幅$224\times 224 \times 3$的图像划分为$14\times 14=196$个patch, 每个patch的大小为$16\times 16\times 3$. 例如, 随机从输入中删除100个这样的patch, 就相当于失去了51%的图像内容.

2. Salient(foreground) PatchDrop

    对于分类器来说, 并不是所有的像素都同样重要, 为了估计显著区域, 作者利用一个自监督的ViT模型DINO, 该模型使用注意力分割图像中的显著目标. 按照这种方法, 作者可以删除一部分有前景信息的patch.

3. Non-Salient(background) PatchDrop

    方法同上, 但是删除具有背景信息的patch.

#### ***鲁棒性分析:***

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_png/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZKU8OFpCAXibu1ILlZyuR8Je4kXeo3jPsyIiauiaLaRqSo0u857eMNzFCQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=80% ></div>

以Random PatchDrop为例, 作者给出五次测试的平均值和标准差, 对于显著性和非显著性PatchDrop, 由于获得遮挡掩模是确定的, 故只给了运行一次的精度值.

从如上结果可以看出, Random PatchDrop 50%的图像使得CNN完全丧失了识别能力, 而DeiT等Transformer-based models均有着较高的识别精度, 更极端的, 当90%图像信息都丢失时, DeiT-B仍然有37%的识别能力. 同样的, 在显著性和非显著性PatchDrop的条件下, ViTs均有着不俗的表现.

#### ***Class Token Perserves Information:***

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0Z2PtibKqjlatGMBmF7aiad1IPClSrG2yEZj2XkOC75oP4AeUAWOjGFiciaA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=80% ></div>

为了更好的了解ViTs模型对遮挡具有鲁棒性的原因, 作者将DeiT-B的不同层的attn. map均进行了可视化, 如上图所示. 可以看出, ViTs的浅层更关注遮挡区域, 较深的层更加关注遮挡以外的信息.

为了探究从浅层到深层的token的变化是否导致了基于遮挡的token不变性, 作者计算了**原始图像**和**遮挡图像**的特征/标tokens的相关系数. 测试ResNet50的相关系数时, 作者使用了logit layer前的feats. 测试ViTs的相关系数时, class token从最后一个Transformer block中获得, 测试结果如下:

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZKUqhxiaU381GJsBpcZYXTOS4lklVo1ib2USYb8Bphia3eS30a1ZK9TNJg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=50%></div>

此外, 作者还可视化了ImageNet中12个子类的相关系数, 可视化结果如下图. 可以看出, 对于不同的类来说, 即使类的目标很小, 比如昆虫, 食物, 鸟类等, 这种现象均存在.

<div align=center><img src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfquW0wTRicWib9wH8mSXSMR0ZjA7zlodvz7LcPhvkxJEzmo4BUp2krWdSAMaEU208rHAEJGDrk4nBNQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" width=50%></div>

### ViTs是否可以同时学习Shape和Texture的相关信息?

Geirhos等人引入了Shape v.s. Texture的假设, 并提出了一种训练框架来增强卷积神经网络的Shape偏差. 类似地, 作者也对ViT模型进行了类似分析, 结果表明, ViTs可获得比CNNs更强的Shape偏差, 但是这种方法会导致对正常图像的精度下降.

为了解决这种问题, 作者另外将shape token引入ViTs结构中, 专门学习shape信息. 具体来说, 作者使用一组不同的tokens在同一ViTs网络结构中分别对Shape和Texture进行建模. 为此, 作者从预训练的强shape-bias的CNN模型中蒸馏shape相关信息, 这种蒸馏方法既保证了合理的分类精度, 也提供了比原始ViTs模型更好的shape偏差.

#### ***<font color=red>shape bias相关介绍</font>***

<div align=center><img src="https://pic1.zhimg.com/v2-1fe3ad62eb7d4c07f230dd3a302663a4_1440w.jpg?source=172ae18b" width=80%></div>

已有研究者表明, ImageNet上预训练的CNN对形状是有偏见的.

举个例子, 如果图片是猫, CNN都会分类正确, 但如果是一只有着大象外皮纹路的猫, CNNs大概率上会识别成大象而不是猫. 目前普遍认为, 人类主要是通过形状来判断物体的, 而CNN更倾向于使用颜色和纹理进行预测, 而非形状.

为了验证上述观点, 严格遵循心理学实验流程设定, 进行了五项实验.

前四项实验中, 作者将16类共160张图白色背景的自然图像做四种不同的变换, 分别交个人类实验者和在ImageNet上预训练的CNNs进行物体识别, 这四种变换包括: 

- 转灰度图: 使用skimage.color.rgb2gray实现;
- 转黑白(轮廓/剪影): 二值化方法+人工检查;
- 边缘提取: 使用matlab实现canny edge extractor;
- 纹理图: 使用每类三张共48张纹理图像, 如动物的毛发或者密集排列的某种物体.

所有的原图和纹理图都可以被人类和CNNs正确的识别; 灰度图像也维持了较高的准确率. 但是到剪影和边缘图像的时候, 人类的识别精度就显现出了明显的优势. 这说明人类可以更好的应对纹理较少或无纹理的图片; 而对于CNNs来说, 这种图像域的偏移(图像空间和训练空间不一样)带来了不小的麻烦.

在第五个实验中, 研究员又使用了形状纹理不一致性(cue conflict). 他们使用迭代式风格迁移的方法, 将纹理图的风格迁移到一些自然图像上, 构造了共16类1280张这种纹理形状不一致的图像, 让人类和CNNs进行识别, 并观察其在形状和纹理之间的偏好. 值得注意的是, 第五个实验不是判断分类正确与否的问题, 只是判断偏好的问题. 下图显示了一些形状纹理不一致的图像: 

<div align=center><img src="https://pic1.zhimg.com/80/v2-326d5456c77cf5e3340a9f394a311ab4_720w.jpg" width=70%></div>

结果可有如下图表表示. 红色圆圈表示了不同类别下人类对于纹理和形状的偏好情况, 不同的蓝色标志表示了不同的CNN结构的偏好情况, 越往左说明对形状的偏好性更强, 反之则对纹理的偏好性较强. 虽然各类图片的情况不甚相同, 但也很好的佐证了上述四个实验的结论.

<div align=center><img src="https://pic1.zhimg.com/80/v2-b3a9711838f6ee6bdb0d495a1376d4f8_720w.jpg" width=70%></div>

对于CNNs的常见解释是, 它通过不断的卷积和池化, 从提取最简单的边角特征, 到不断融合这些浅层特征, 从而获得更为复杂的表示. 现在看来, 形状并不是CNNs的主要判断依据, 颜色和纹理才是. 这就是所谓的对纹理和颜色的偏好.

实验证明, 提高CNNs对形状的偏好可以使得提取的特征更加鲁棒, 可以提升分类等任务的准确度. 下面简单介绍三种方法.

- **风格迁移训练**
  - Geirhos等人对ImageNet数据集使用AnaIN方法进行随机风格迁移, 构成新的Stylized-ImageNet数据集, 原数据集和新数据集分别记为IN和SIN. 数据集样例如下: 
    <div align=center><img src="https://pic3.zhimg.com/80/v2-c4e363bd0100384e89fe6a963b6061c2_720w.jpg" width=70%></div>
  - 通过在IN和SIN上的混合训练以及微调过程, 得到Shape-ResNet模型对于形变失真图像的鲁棒性增强, 在ImageNet数据及上的分类准确性和Pascal VOC上目标检测的准确性都有所提升.

- **域对抗训练**
  - Brochu等人验证了Geirhos等人工作的有效性, 并进一步使用域对抗训练的方法解决这一问题.
  - 该方法通过域分类器(domain classifier)和梯度反向层(gradient reversal layer)实现对于不同域一致性特征的学习.
<div align=center><img src="https://pic1.zhimg.com/80/v2-620dbf74d9a51c3814d4174e52e22cf4_720w.jpg" width=70%></div>

- **基于拼图的自监督学习**

Reference:
- [谷歌研究员：Transformer那些有趣的特性](https://mp.weixin.qq.com/s?src=11&timestamp=1622772800&ver=3109&signature=xxtu8aeppESTqpe1jCLbPKBtNoKIBW0gV0PZw7eT2RyoFjzbur0OJBg8UmKrOtWRgW3Ab7rG7UnGgK4WXsjI7AeJAp0lYfyuqpLZbeptuym80J-KKqveQnkPYveVuqcX&new=1)