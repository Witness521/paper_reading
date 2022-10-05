# diffusion model在图像修复上的应用

### 1. diffusion model简述

<img src="https://pic3.zhimg.com/80/v2-42181e6098a90635a05cfeb1c1091afe_720w.webp" alt="img" style="zoom:80%;" />

​		一句话概括diffusion model，即存在一系列高斯噪声（ T 轮），将输入图片 x0 变为纯高斯噪声 $x_T$ 。而我们的模型则负责将 $x_T$ 复原回图片 $x_0$ 。这样一来其实diffusion model和GAN很像，都是给定噪声 $x_T$ 生成图片$x_0$  ，但是要强调的是，这里噪声 $x_T$  与图片$x_0$ 是**同维度**的。

<img src="H:\files\python_file\paper_reading\image inpainting\pic\diffusion_model1.png" alt="image-20221005212520483" style="zoom:50%;" />

**生成式模型**本质上是学习一组概率分布，通过$p_\theta$ 学习$p_{data}$ 的分布实现图像的生成

 

