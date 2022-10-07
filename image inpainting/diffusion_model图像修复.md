# diffusion model在图像修复上的应用

### 1. diffusion model简述

<img src="https://pic3.zhimg.com/80/v2-42181e6098a90635a05cfeb1c1091afe_720w.webp" alt="img" style="zoom:80%;" />

​		一句话概括diffusion model，即存在一系列高斯噪声（ T 轮），将输入图片 x0 变为纯高斯噪声 $x_T$ 。而我们的模型则负责将 $x_T$ 复原回图片 $x_0$ 。这样一来其实diffusion model和GAN很像，都是给定噪声 $x_T$ 生成图片$x_0$  ，但是要强调的是，这里噪声 $x_T$  与图片$x_0$ 是**同维度**的。[链接](https://yinglinzheng.netlify.app/diffusion-model-tutorial/) [b站链接](https://www.bilibili.com/video/BV1rW4y1Y7M5/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=b5a07bd11a65b520a98c9855e8d78245) 

<img src="H:\files\python_file\paper_reading\image inpainting\pic\diffusion_model1.png" alt="image-20221005212520483" style="zoom:50%;" />

**生成式模型(diffusion model)**本质上是学习一组概率分布，通过$p_\theta$ 学习$p_{data}$ 的分布实现图像的生成

 而**从单个图像样本来**看这个过程，扩散过程q就是不断往图像上加噪声直到图像变成一个纯噪声，逆扩散过程p就是从纯噪声生成一张图像的过程。更具体地说，扩散模型是一种**隐变量模型**（latent variable model）,使用马尔科夫链（Markov chain）映射到latent space。通过马尔科夫链，在每一个时间步t中逐渐将噪声添加到数据$x_i$中以获得后验概率$q(x_{1:T}|x_0)$  ，其中$x_1,...,x_T$ 代表输入的数据同时也是latent space。也就是说diffusion model中的latent space与输入数据具有**相同的维度**。

<img src="https://yinglinzheng.netlify.app/diffusion-model-tutorial/assets/image-20220926183806234.png" alt="Trulli" style="zoom:80%;" />

### **2. 扩散过程**

Diffusion model分为正向的扩散过程和反向的逆扩散过程，扩散是加噪声的过程，逆扩散是去噪的过程。下图为扩散过程，从 x0 到最后的 xT 就是一个马尔可夫链，表示状态空间中经过从一个状态到另一个状态的转换的随机过程。

<img src="https://pic2.zhimg.com/v2-d96d108705d61e59b76b341534986d39_r.jpg" alt="img" style="zoom: 40%;" />

给定真实图片样本 x0∼q(x) ，diffusion 前向过程通过 T次累计对其添加高斯噪声，得到 x1,x2,...,xT，如下图的 q 过程。每一步的大小是由一系列的高斯分布方差的超参数 {βt∈(0,1)}t=1T来控制的。 前向过程由于每个时刻 t 只与 t−1时刻有关，所以也可以看做马尔科夫过程：

<img src="H:\files\python_file\paper_reading\image inpainting\pic\扩散过程公式.png" alt="image-20221006112109472" style="zoom:80%;" />

若要求任意一个时刻的$x_t$ 不需要从$X_0$ 开始推导，可以根据$x_0$和$\beta_{t}$直接计算$x_t$ 时刻的 $q(x_t|x_0)$ 

<img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20221006115507806.png" alt="image-20221006115507806" style="zoom:80%;" />

<img src="H:\files\python_file\paper_reading\image inpainting\\pic\image-20221006115806450.png" alt="image-20221006115806450" style="zoom:80%;" />

当T$\rightarrow$ $∞$ 的时候，$x_T$ 服从标准正态分布，这时候$x_T$ 也就是一个完全的噪声，与$x_0$ 几乎没有什么关系。 

### 3. 逆扩散过程

<img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20221006121956157.png" alt="image-20221006121956157" style="zoom: 80%;" />
