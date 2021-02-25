[TOC]

# Oceans

## Papers

### Unsorted

##### Mixture Density Networks

<img src="/Users/shuffle_new/Library/Application Support/typora-user-images/image-20210223235049873.png" alt="image-20210223235049873" style="zoom:25%;" />

用mixture gaussian来model conditional density. x是condition， $p(\mathbf{t} \mid \mathbf{x})=\sum_{i=1}^{m} \alpha_{i}(\mathbf{x}) \phi_{i}(\mathbf{t} \mid \mathbf{x})$, with $\phi_{i}(\mathbf{t} \mid \mathbf{x})=\frac{1}{(2 \pi)^{c / 2} \sigma_{i}(\mathbf{x})^{c}} \exp \left\{-\frac{\left\|\mathbf{t}-\boldsymbol{\mu}_{i}(\mathbf{x})\right\|^{2}}{2 \sigma_{i}(\mathbf{x})^{2}}\right\}$. NN的input是x，output是The parameters $\alpha_i(\mathbf{x}), \boldsymbol{\mu}_{i}(\mathbf{x}), \sigma_{i}(\mathbf{x})$. Training objective function is set to minimize the log likelihood function。

##### The Kernel Mixture Network: A Nonparametric Method for Conditional Density Estimation of Continuous Random Variables



##### Conditional Density Estimation with Neural Networks: Best Practices and Benchmarks

文章里提出的，在MDN和KMN上的基础上，NN+Loglikelihood。提出了两个新的东西：

1. Variable Noise as Smoothness Regularization，在condition和dependent上加permutation，$\tilde{\boldsymbol{x}}_{n}=\boldsymbol{x}_{n}+\boldsymbol{\xi}_{x} \text { and } \tilde{\boldsymbol{y}}_{n}=\boldsymbol{y}_{n}+\boldsymbol{\xi}_{y}$，这个思路跟咱们有点像
2. Data Normalization，这个倒没啥，ML的这群人早就弄过了

后面实验里用了好多evaluation metrics，可以参考一下

综述写的很好，比较全面。综述里提到传统统计的conditional density estimation可以分为parametric和non parametric的方法。很好理解：

![image-20210225215521032](/Users/shuffle_new/Library/Application Support/typora-user-images/image-20210225215521032.png)

parametric的方法问题主要在于不够灵活，要预先假设一个conditional distribution的family。non parametric的方法，除了不condition时候的KDE的问题（不够灵活、bandwidth要合适选取等等）之外，有可能$\hat{p}(\boldsymbol{x})$接近0的时候，$\hat{p}(\boldsymbol{y} \mid \boldsymbol{x})=\frac{\hat{p}(\boldsymbol{x}, \boldsymbol{y})}{\hat{p}(\boldsymbol{x})}$给出的conditional density也不够稳定。

### Normalizing Flows

##### Variational inference with normalizing flows

论文里有好多 Variational Inference 的术语，还没完全看懂。

TODO：VI的condition和我们的condition是不是一样的东西

##### DENSITY ESTIMATION USING REAL NVP

没找到跟condition有关的东西，大概也是用normalizing flow，但是改了一点点network的东西。做的是图像生成和图像interpolation。

##### LEARNING LIKELIHOODS WITH CONDITIONAL NORMALIZING FLOWS

这个就是conditional normalizing flow。

![image-20210225214629880](/Users/shuffle_new/Library/Application Support/typora-user-images/image-20210225214629880.png)

做的是image superresolution。提到了一个叫Split Prior的技术，把condition给拆开，降低condition的难度，似乎值得一看。



## Techniques

##### Normalizing Flow

大概意思是，用一系列的invertible functions（这些function也是NN实现的，只是加了一个invertible的额外要求），把一个简单的distribution给转化到target distribution。mathematically, $\boldsymbol{z}$ is sampled from a simple distribution, $f_1,f_2,\ldots,f_n$ are invertible functions. We expect $\boldsymbol{x}=f_n(f_{n-1}(\cdots f_1(\boldsymbol{z})))$ is close to a target distribution (the emprical distribution of a dataset). The goodness of applying invertible functions is we can give the density of any $\boldsymbol{x}$. Then maximum log likelihood is set as the optimization objective. 能exactly evaluate likelihood和output density。计算output density的cost涉及计算inverse jacobian matrix，cost比较高。一些论文就在搞计算简单的invertible functions，比如coupling layer技术。

借用LEARNING LIKELIHOODS WITH CONDITIONAL NORMALIZING FLOWS里的公式，NF做的是下面的事：

![image-20210225215018198](/Users/shuffle_new/Library/Application Support/typora-user-images/image-20210225215018198.png)

NF似乎可以理解成，只用了gan里面的generator，并且对这个generator加了invertible的要求。gan的discriminator则由maximum log likelihood来完成。（似乎感觉，对Pierre model这个不太复杂的分布，用normalizing flow足够灵活且稳定。但condition distribution怎么learn还是个问题，顺着Conditional Normalizing Flow关键词可以接着找）

https://www.youtube.com/watch?v=i7LjDvsLWCg



##### VAE



##### Variational Inference

https://www.zhihu.com/question/41765860

https://zhuanlan.zhihu.com/p/49401976

##### 为什么要simulate from conditional distributions：

1. Arrival data上real time decision
2. 不一定只是说arrival data，比如看的一些conditional density estimation的paper，一个dataset纽约出租车，condition是上车的位置和时间，dependent是下车的位置。

##### Auto-regressive model

component by component生成一个sequence，比如语音就很合适。

arrival data似乎很适合component by component？

https://www.youtube.com/watch?v=uXY18nzdSsM 最前面讲了几分钟，肯定有完全讲auto-regressive model的lecture

## Keywords

Conditional Normalizing Flow

Split Prior

Autoregressive generative model

