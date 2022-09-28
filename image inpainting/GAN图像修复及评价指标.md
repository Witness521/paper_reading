#### 1. GAN在图像修复过程中的缺陷：

<img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20220914213119722.png" alt="image-20220914213119722" style="zoom:67%;" />



<img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20220914213300754.png" alt="image-20220914213300754" style="zoom:67%;" />

#### 2. 提出的改进方法/模型：

1. 将非监督学习的GAN和监督学习的CNN进行结合成为DCGAN（深度卷积生成式对抗网络）-- 效果一般

​    DCGAN中所有的池化层使用卷积层替换，**生成器模型**中使用转置卷积（反卷积）替换最大池化层，**判别器模型**中使用步幅卷积(strided convolution)替换最大池化层

<img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20220914214621061.png" alt="image-20220914214621061" style="zoom:80%;" />



2. Wasserstein 生成式对抗网络(WGAN)

   WGAN解决了GAN的三点问题：

- 解决了生成式对抗网络训练不稳定，生成器、判别器的训练程度无法准确衡量的问题。
- 解决了训练崩溃(collapse mode)问题，保证生成样本的多样性。
- 解决了训练过程中没有确切的目标函数指示网络训练进程的问题。设定目标函数越小表示网络训练程度越好，生成器生成的图像更加清晰逼真。

WGAN相对于GAN做的改进

(1)生成器、判别器的目标函数根据 Wasserstein 距离原理不再取 log；Wasserstein距离衡量两个概率分布之间距离的度量方法，适用于两个分布之间没有交集的情况。

(2)判别器最后一层去掉 Sigmoid 函数；

(3)每次更新判别器网络后，参数范围重置为 $w\in[-c,c]$
(4)更改基于动量的优化算法 Adam 为 RMSProp。



#### 3. 图片质量评价指标：

评价指标：[图片质量评价指标](https://zhuanlan.zhihu.com/p/50757421) 

1.  **PSNR (Peak Signal-to-Noise Ratio) 峰值信噪比**   PSNR的值越大越好

​      均方误差(MSE)：<img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20220916152602673.png" alt="image-20220916152602673" style="zoom:67%;" />

​      <img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20220916152849432.png" alt="image-20220916152849432" style="zoom:67%;" />

​		code:`psnr = skimage.measure.compare_psnr(im1, im2, 255)`

2. **SSIM (Structural SIMilarity) 结构相似性**  SSIM值越大越好

​	   SSIM是一种用来**衡量图片相似度**的指标，也可用来判断图片压缩后的质量。

   计算相关代码： `skimage.measure.compare_ssim(im1, im2, data_range=255)`

<img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20220916154035063.png" alt="image-20220916154035063" style="zoom:80%;" />

#### 4. GAN的评价指标IS和FID:

[GAN的评价指标](https://blog.csdn.net/jackzhang11/article/details/104995524)

评价GAN的好坏，可以从一下两方面进行考量：

1. 生成的图像是否清晰，清晰则代表生成的图片质量高。
2. 生成的图像是否具有多样性，即每个类别生成图像的数量尽可能相等。

**Inception Score（IS）**IS指标使用较少：IS值越大越好 <img src="H:\files\python_file\paper_reading\image inpainting\pic\image-20220916161244959.png" alt="image-20220916161244959" style="zoom:67%;" />



**Frechet Inception Distance（FID）**  FID越小越好

FID考虑的更多的是生成的图像与真实图像之间的联系。该算法也是通过Inception模型进行计算的。不同的是，FID拿掉了Inception模型最后的一个用于**分类全连接层**，将前面一层的2048维向量进行输出。在这里，Inception不再进行分类，而是进行特征提取，得到的2048维向量，每一个维度都表示着某种特征。

FID表示的是**生成图像的特征向量与真实图像的特征向量之间的距离**，该距离越近，表明生成模型的效果越好，即图像的清晰度高，且多样性丰富。

FID相对于IS来说有着更好的鲁棒性，因此在评价GAN模型中受用面更广一些。



