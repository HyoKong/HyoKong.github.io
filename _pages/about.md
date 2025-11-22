---
permalink: /
title: ""
excerpt: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

{% if site.google_scholar_stats_use_cdn %}
{% assign gsDataBaseUrl = "https://cdn.jsdelivr.net/gh/" | append: site.repository | append: "@" %}
{% else %}
{% assign gsDataBaseUrl = "https://raw.githubusercontent.com/" | append: site.repository | append: "/" %}
{% endif %}
{% assign url = gsDataBaseUrl | append: "google-scholar-stats/gs_data_shieldsio.json" %}

<span class='anchor' id='about-me'></span>

# Hi there!

Welcome to the website of **Hanyang Kong (孔晗旸)**. 🌐

I am honored to be pursuing my Ph.D. at the [xML Lab @ NUS](https://sites.google.com/view/xml-nus), advised by [Prof. Xinchao Wang](https://sites.google.com/site/sitexinchaowang/) since August 2021. My academic journey in computer science began at **Xi’an Jiaotong University**, where I earned my master’s degree under the mentorship of [Prof. Qingyu Yang](https://gr.xjtu.edu.cn/web/yangqingyu/1). 🎓

My research is centered on **AIGC (Artificial Intelligence-Generated Content)** and **LLMs (Large Language Models)**, with a growing emphasis on **3D/4D Vision-Language-Action (VLA) models**—empowering agents to understand, reason, and act in both static and dynamic real-world environments with spatial and temporal depth.

**Key research interests include:**
- 📊 **3D Generation and Estimation**: Advancing digital modeling for realistic scene understanding.  
- 💡 **Diffusion Models and LLM Applications**: Exploring generative AI for both creative and practical purposes.  
- 🌍 **3D/4D Vision-Language-Action (VLA) Models**: Developing embodied AI systems capable of perceiving, interpreting, and interacting with complex physical real-world environments.

Thank you for visiting!

I’m open to employment opportunities for **Research Scientist/Engineer** roles starting in **2026**, preferably in **Singapore**, **Beijing**, or **Shanghai**.



# 🔥 News
- *2025.06*: &nbsp;🎉🎉 Our paper [*RogSplat: Robust Gaussian Splatting via Generative Priors*](https://openaccess.thecvf.com/content/ICCV2025/papers/Kong_RogSplat_Robust_Gaussian_Splatting_via_Generative_Priors_ICCV_2025_paper.pdf) was accepted by ICCV’25. 
- *2025.02*: &nbsp;🎉🎉 Our paper [*Generative Sparse-View Gaussian Splatting*](https://openaccess.thecvf.com/content/CVPR2025/papers/Kong_Generative_Sparse-View_Gaussian_Splatting_CVPR_2025_paper.pdf) was accepted by CVPR’25. 
- *2024.10*: &nbsp;🎉🎉 Our paper [*EDGS: Efficient Gaussian Splatting for Monocular Dynamic Scene Rendering via Sparse Time-Variant Attribute Modeling*](https://arxiv.org/abs/2502.20378) was accepted by AAAI’25. 
- *2024.07*: &nbsp;🎉🎉 Our paper [*DreamDrone: Text-to-Image Diffusion Models are Zero-shot Perpetual View Generators*](https://arxiv.org/abs/2312.08746) was accepted by ECCV’24. 
<!-- - *2023.12*: &nbsp;🎉🎉 🌟Our new work, *DreamDrone: Text-to-Image Diffusion Models are Zero-shot Perpetual View Generators*, is released! Check our [paper](https://arxiv.org/abs/2312.08746) and [code](https://github.com/HyoKong/DreamDrone.git)! Also, please feel free to try our online [huggingface demo](https://huggingface.co/spaces/imsuperkong/dreamdrone)! -->
- *2023.07*: &nbsp;🎉🎉 Our paper [*Priority-centric human motion generation in discrete latent space*](https://arxiv.org/abs/2308.14480) was accepted by ICCV’23. 

# 📝 Publications 

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">Arxiv 2025</div><img src='images/worldwarp-teaser.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">


***WorldWarp: Propagating 3D Geometry with Asynchronous Video Diffusion***

**Hanyang Kong**, Xingyi Yang, Xiaoxu Zheng, Xinchao Wang

<!-- [**Project page**](https://hyokong.github.io/gsgs-page/), [**Paper**](ttps://openaccess.thecvf.com/content/CVPR2025/papers/Kong_Generative_Sparse-View_Gaussian_Splatting_CVPR_2025_paper.pdf) <strong><span class='show_paper_citations' data='DhtAFkwAAAAJ:ALROH1vI_8AC'></span></strong> -->
- **Asynchronous Video Diffusion:** Adapts full-sequence video diffusion model into an asynchronous diffusion process for infinite, pose-guided scene generation from a single image.
- **Warped Visual Hints:** Uses forward-warped 3DGS renderings as strong, explicit conditions to direct the synthesis of future frames.
- **Online Geometric Cache:** Ensures cross-chunk consistency by dynamically optimizing the underlying 3D geometry.
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ICCV 2025</div><img src='images/rogsplat-teaser.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">


[***RogSplat: Robust Gaussian Splatting via Generative Priors***](https://openaccess.thecvf.com/content/ICCV2025/papers/Kong_RogSplat_Robust_Gaussian_Splatting_via_Generative_Priors_ICCV_2025_paper.pdf)

**Hanyang Kong**, Xingyi Yang, Xinchao Wang

<!-- [**Project page**](https://hyokong.github.io/gsgs-page/), [**Paper**](ttps://openaccess.thecvf.com/content/CVPR2025/papers/Kong_Generative_Sparse-View_Gaussian_Splatting_CVPR_2025_paper.pdf) <strong><span class='show_paper_citations' data='DhtAFkwAAAAJ:ALROH1vI_8AC'></span></strong> -->
- **Make 3DGS Work in the Wild:** RogSplat fixes real-world issues like occlusion and motion blur.
- **Smart Outlier Cleanup:** Detects and inpaints corrupted regions with a generative refiner.
- **Reliable in Real Scenes:** Outperforms prior methods on complex real-world datasets.
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">CVPR 2025</div><img src='images/gsgs-teaser.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">


[***Generative Sparse-View Gaussian Splatting***](https://openaccess.thecvf.com/content/CVPR2025/papers/Kong_Generative_Sparse-View_Gaussian_Splatting_CVPR_2025_paper.pdf)

**Hanyang Kong**, Xingyi Yang, Xinchao Wang

<!-- [**Project page**](https://hyokong.github.io/gsgs-page/), [**Paper**](ttps://openaccess.thecvf.com/content/CVPR2025/papers/Kong_Generative_Sparse-View_Gaussian_Splatting_CVPR_2025_paper.pdf) <strong><span class='show_paper_citations' data='DhtAFkwAAAAJ:ALROH1vI_8AC'></span></strong> -->
- **Turn Sparse Views into Rich 3D:** Boosts 3D/4D Gaussian splatting with diffusion-based novel views.
- **Geometry-Aware Consistency:** Enforces structural alignment via semantic correspondences.
- **More with Less:** Matches dense data models using only a few input images.
</div>
</div>



<div class='paper-box'><div class='paper-box-image'><div><div class="badge">AAAI 2025</div><img src='images/edgs-pipe.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">


[***Efficient Gaussian Splatting for Monocular Dynamic Scene Rendering via Sparse Time-Variant Attribute Modeling***](https://arxiv.org/abs/2502.20378)

**Hanyang Kong**, Xingyi Yang, Xinchao Wang

<!-- [**Project page**](https://hyokong.github.io/edgs-page/), [**Code**](https://github.com/HyoKong/DreamDrone.git), [**Arxiv**](https://arxiv.org/abs/2502.20378) <strong><span class='show_paper_citations' data='DhtAFkwAAAAJ:ALROH1vI_8AC'></span></strong> -->
- **Voxelized Time-Variant 3DGS:** Introduces deformable Gaussian splatting with unsupervised attribute filtering.
- **Kernel-Based Motion Flow:** Formulates scene deformation using sparse, interpretable flow.
- **Faster, Better Rendering:** Achieves faster rendering with superior visual quality.
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ECCV 2024</div><img src='images/dreamdrone.png' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[***DreamDrone: Text-to-Image Diffusion Models are Zero-shot Perpetual View Generators***](https://arxiv.org/abs/2312.08746.pdf) 
<!-- <img src='https://img.shields.io/github/stars/HyoKong/DreamDrone.svg?style=social&label=Star' alt="sym" height="100%"> -->

**Hanyang Kong**, Dongze Lian, Michael Bi Mi, Xinchao Wang

<!-- [**Project page**?](https://hyokong.github.io/dreamdrone-page/), [**Huggingface demo**](https://huggingface.co/spaces/imsuperkong/dreamdrone), [**Code**](https://github.com/HyoKong/DreamDrone.git), [**Arxiv**](https://arxiv.org/abs/2312.08746) <strong><span class='show_paper_citations' data='DhtAFkwAAAAJ:ALROH1vI_8AC'></span></strong> -->
- **Zero-shot, Training-Free Scene Creation:** Generates perceptual scenes directly from text, without specific training for each scene.
- **Click-Guided Dreamscapes Navigation:** Allows drone flight control through point selection, offering a visually immersive experience.
- **Resource-Efficient Scene Generation:** Omits the need for a 3D point cloud, enabling faster scene creation with lower computational demand.
</div>
</div>

<div class='paper-box'><div class='paper-box-image'><div><div class="badge">ICCV 2023</div><img src='images/t2m.jpg' alt="sym" width="100%"></div></div>
<div class='paper-box-text' markdown="1">

[***Priority-Centric Human Motion Generation in Discrete Latent Space***](https://arxiv.org/pdf/2308.14480.pdf)

**Hanyang Kong**, Kehong Gong, Dongze Lian, Michael Bi Mi, Xinchao Wang

<!-- [**Project**](https://scholar.google.com/citations?view_op=view_citation&hl=zh-CN&user=DhtAFkwAAAAJ&citation_for_view=DhtAFkwAAAAJ:ALROH1vI_8AC) <strong><span class='show_paper_citations' data='DhtAFkwAAAAJ:ALROH1vI_8AC'></span></strong> -->
- Text-to-motion generation in descrete latent space. 
- Priority-centric diffusion scheme for the discrete diffusion model.
</div>
</div>

<!-- - [Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet](https://github.com), A, B, C, **CVPR 2020** -->

<!-- # 🎖 Honors and Awards
- *2021.10* Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet. 
- - *2015-2019(B.Eng.)*:  -->

# 📖 Educations
- *2021.08 - present*: Ph.D. candidate in College of Design and Engineering, National University of Singapore.
- *2017.08 - 2020.06*: M.Eng. in Faculty of Electronic and Information Engineering, Xi'an Jiaotong University. 
- *2013.08 - 2017.06*: B.Eng. in Faculty of Electrical Engineering and Automation, Hefei University of Technology. 


<!-- # 💬 Invited Talks
- *2021.06*, Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet. 
- *2021.03*, Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus ornare aliquet ipsum, ac tempus justo dapibus sit amet.  \| [\[video\]](https://github.com/) -->

<!-- # 💻 Internships
- *2019.05 - 2020.02*, [Lorem](https://github.com/), China. -->
