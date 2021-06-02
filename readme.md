# XMU数据挖掘大作业——电影推荐系统

GoatWu

## 一、基础知识

### 1. 基于内容的推荐系统

**已知**：电影内容矩阵 $X$、电影评分矩阵 $Y$：

![截屏2021-03-03 下午7.48.48](https://goatwu.oss-cn-beijing.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2021-03-03%20%E4%B8%8B%E5%8D%887.48.48.png)

**求出**：用户喜好矩阵 $\theta$。
$$
J(\theta)=\frac{1}{2}\sum_{j=1}^{u}\sum_{i:r(i,j)=1}\left(\left(\theta^{(j)}\right)^Tx^{(i)}-y^{(i,j)}\right)^2+\frac{\lambda}{2}\sum_{i=1}^{m}\sum_{k=1}^n\left(x_k^{(i)}\right)^2
$$
**其中**：

- $u$：用户数量
- $r(i,j)$：用户 $j$ 对电影 $i$ 进行了评分则为 $1$，否则为 $0$.
- $\theta^{(j)}$：$j$ 用户对不同类型电影喜好的向量
- $x(i)$：$i$ 电影的不同内容成分向量
- $y^{(i,j)}$：用户 $j$ 对电影 $i$ 的真实评分
- $n$：特征数量（图中电影特征数量为 $3$）。最后一大项为「正则化项」，调整 $\lambda$ 防止 $\theta$ 过拟合

> $\left(\theta^{(j)}\right)^Tx^{(i)}$ 即为预测中，用户对电影的评分；
>
> $\left(\theta^{(j)}\right)^Tx^{(i)}-y^{(i,j)}$ 即为预测评分的误差

**问题**：

电影内容矩阵 $X$ 难以得出。

### 2. 协同过滤

#### 2.1. 算法

**已知：**

1. 电影评分矩阵

   ![截屏2021-03-03 下午7.46.42](https://goatwu.oss-cn-beijing.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2021-03-03%20%E4%B8%8B%E5%8D%887.46.42.png)

2. 用户喜好矩阵 $\theta$

   ![截屏2021-03-03 下午7.46.48](https://goatwu.oss-cn-beijing.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2021-03-03%20%E4%B8%8B%E5%8D%887.46.48.png)

**要求出：**

对应的电影内容矩阵 $X$

![截屏2021-03-03 下午7.48.48](https://goatwu.oss-cn-beijing.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2021-03-03%20%E4%B8%8B%E5%8D%887.48.48.png)

**从而**：预测出每个用户对于没有看过电影的评分。

电影内容矩阵代价函数：
$$
J(X)=\frac{1}{2}\sum_{j=1}^{m}\sum_{i:r(i,j)=1}\left(\left(\theta^{(j)}\right)^Tx^{(i)}-y^{(i,j)}\right)^2+\frac{\lambda}{2}\sum_{i=1}^{m}\sum_{k=1}^n\left(\theta_k^{(j)}\right)^2
$$
**其中**：

- $m$：电影数量
- 其他和前面相同

**发现**：两种方法第一项一样，因此可以合并来求：
$$
J(X,\theta)=\frac{1}{2}\sum_{(i,j):r(i,j)=1}\left(\left(\theta^{(j)}\right)^Tx^{(i)}-y^{(i,j)}\right)^2+\frac{\lambda}{2}\sum_{i=1}^{m}\sum_{k=1}^n\left(x_k^{(i)}\right)^2+\frac{\lambda}{2}\sum_{i=1}^{m}\sum_{k=1}^n\left(\theta_k^{(j)}\right)^2
$$

#### 2.2. 协同过滤的优缺点

1. 优点
   - 能够根据各个用户的历史信息推断出商品的质量
   - 不需要对商品有任何专业领域
2. 缺点
   - 冷启动问题：有前提条件，用户已经观看了一部分电影，我们具有了用户信息。
   - gray sheep：一个用户如果没有相似的用户，就不能成功推荐
   - 复杂度随商品数量和用户数量的增加而增加
   - shilling attack：给自己刷高分、别人刷低分

