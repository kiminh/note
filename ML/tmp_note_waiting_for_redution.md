1. Relu & Sigmoid

   - Advantage:
    1. Sigmoid: 不会 blowing up(爆炸) 激活
    2. Relu: 不会使梯度消失
    3. Relu: 比 Sigmoid 有更多有效的计算, 因为只需要对 $(0,x)$ 区间的值进行操作
    4. Relu: 收敛性更好([Krizhevsky et al](http://www.cs.toronto.edu/~fritz/absps/imagenet.pdf))
   - Disadvantage:
    1. 
   - source: [What are the advantages of ReLU over sigmoid function in deep neural networks?](https://stats.stackexchange.com/questions/126238/what-are-the-advantages-of-relu-over-sigmoid-function-in-deep-neural-networks)

2. 注意: 神经网络不一定始终比特征组合好, 但确实可以提供适用于很多情形的灵活替代方案。

3. some details:
    1. 梯度消失: 较低层(更接近输入) 的梯度可能会变得非常小。 在深度网络中, 计算这些梯度可能设计许多小项的乘积, 当较低层的梯度逐渐消失到 $0$ 时, 这些层的训练速度会非常缓慢, 甚至不再训练。 ReLU 激活函数可以有助于防止梯度消失
    2. 梯度爆炸: 如果网络中的权重过大, 则较低层的梯度会设计许多大项的乘积, 该种情况下, 梯度会爆炸(梯度过大导致难以收敛)。
    3. ReLU 单元消失: 一旦 `ReLU` 单元的加权和低于 $0$, `ReLU` 但愿就可能会停滞(即该节点的输出对网络输出没有任何贡献的 $0$ 激活)。 由于梯度的来源被切断, `ReLU` 的输入可能无法作出足够的改变来使加权和恢复到 $0$ 以上(降低学习速率有助于防止 `ReLU` 单元丢失)。
    4. 丢弃正则化: 被称为`丢弃`的另一种形式的正则化, 可用于神经网络。 在梯度下降发的每一步中随机丢弃一些网络但愿, 丢弃的越多, 正则化效果越强。
        - 0.0: 无丢弃正则化。
        - 1.0: 丢弃所有内容。模型学习不到任何规律。

4. $Sofomax$ 方程式:
$${p(y=j|x)}=\frac{e^{(w_j^Tx+b_j)}}{\sum_{k\in{K}}e^{(w_k^Tx + b_k)}}$$
  > 本质上是将罗辑回归公式延伸到多类别。
  - **完整 Softmax:**
  - **候选采样:** 针对所有正类别标签计算概率, 但仅针对附类别标签的随机样本计算概率。
  > 类别数量较少是, 完整的 $Softmax$ 代价很小, 类别的增加会增加其代价。 候选采样可以提高处理具有大量类别问题的效率。  
  > $Softmax$ 假设每个样本只是一个类别的成员, 但一些样本可以同时是多个类别的成员, 对于此类情况:
  > - 不能使用 $Softmax$
  > - 必须依赖多个罗辑回归

5. 泊松回归(Poisson Regression) & Softmax 回归(Softmax Regression)

    1. 泊松回归: 假设向量 $x$ 表示引起时间发生的因素, 向量 $\theta$ 表示因素的权重, 则使用 $h_\theta(x)=exp(\theta^{\top}x)$ 表示时间发生的次数的期望。 $\theta^{\top}x$ 每增加一个单位, 将导致事件发生次数的期望值翻倍。此时因变量与自变量近似满足泊松分布 $y^i=\pi(h_\theta(x^i))$。  
    似然函数:
    $$L(y|x;\theta)=\prod_{i=1}^m{P(y^i|x^i;\theta)}=\prod_{i=i}^m\frac{e^{h_\parallel(x^{x^i})}h_\theta(x^i)^{y^i}}{y^i!}$$
    对数似然函数:
    $$\ln{L(y|x;\theta)=\sum_{i=1}^m(-h_\theta(x^i)+y^i\ln{h_\theta(x^i)-\ln{y^i!}})}$$
    定义损失函数:
    $$J(\theta)=\sum_{i=1}^m(-h_\theta(x^i)+y^i\ln{h_\theta(x^i)})$$
    要使似然函数最大, 需要似损失函数最小。 使用损失函数的极小值代替最小值:
    $$\frac{\partial}{\partial{\theta_j}}J(\theta)=-\frac{1}{m}\sum_{i=1}^m(-h_\theta(x^i)x^j+y_i\frac{1}{h_\theta(x^i)}h_\theta(x^i)x^j),j=0,1,\dots,n$$
    简化后:
    $$\frac{\partial}{\partial\theta}J{\theta}=\frac{1}{m}\sum_{i=1}^m(h_\theta(x^i)-y^i)x_j^i,j=0,1,\dots,n$$
    使用梯度下降发迭代求解:
    $$\theta_j^{k+1}=\theta_j^k-\alpha\frac{\partial}{\partial\theta_j}J(\theta),j=0,1,\dots,n,\alpha为学习率$$
    2. $Softmax$ 回归: 之前的罗辑回归模型可以解决二分类分体。 将二分类推广为 $k$ 分类问题有: 在楼继回归中, 因变量 $y\in{\left\{ 1,2,\dots,k \right\}}$ 分别对应 $k$ 个分类。 $Softmax$ 回归假定因变量服从 $\Phi_1,\Phi_2,\dots,\Phi_k$ 的多项分布, 即 $y(i)\sim{Mult(\Phi_1,\Phi_2,\dots,\Phi_k)}$。其中:
    $$\phi_i=\frac{\exp(\theta_i^\top{x})}{1+\sum_{l=1}^{k-1}\exp()\theta_l^\top{x}},i=1,2,\dots,k-1$$
    参数 $\Phi_k$ 是冗余的, 利用概率之和等于 $1$ 的条件得:
    $$\phi_k=1-\sum_{i=1}^{k-1}\phi_i=\frac{1}{1+\sum_{l=1}{k-1}\exp(\theta_l^\top{x})}$$
    同事定义:
    $$h(x)=\begin{bmatrix}
       \phi_1\phi2\dots\phi_{k-1} 
    \end{bmatrix}^\top$$
    $$\theta=\begin{bmatrix}
       \theta_1\theta_2\dots\theta_{k-1} 
    \end{bmatrix}^\top$$
    $$\theta_i=\begin{bmatrix}
        \theta_{i0}\theta_{i1}\dots\theta_{in}
    \end{bmatrix}^\top,i=1,2,\dots,k$$
    $\Phi$ 具有如下性质:
    $$\frac{\partial\ln\phi_i}{\partial\theta_{pq}}=(1-\phi_p)x_q,i=p$$
    $$\frac{\partial\ln\phi_i}{\partial\theta_{pq}}=-\phi_px_q,i=1,2,\dots,k$$
    尤其注意: 上述性质对 $i=k$ 依然成立。
    参数 $\theta$ 的极大似然估计, 似然函数:
    $$L(y|x;\theta)=\prod_{t=1}^mP(y^t|x^t;\theta)=\prod_{t=1}^m\prod_{t=1}^k\phi_i^{1\{y^t=i\}}$$
    其中, $expression$ 定义如下: 当 $expression$ 为真时, 函数值为 $1$; 否则为 $0$。 进一步简化:
    对数似然函数:
    $$\ln{L(y|x;\theta)}=\sum_{t=1}^m\sum_{i=1}^k1\{y^y=i\}\ln{\phi_i}$$
    定义损失函数:
    $$J(\theta)=-\frac{1}{m}\sum_{t=1}^m\sum_{i=1}^k1\begin{Bmatrix}
        y^t=i
    \end{Bmatrix}\ln{\phi_i}$$
    要使似然函数最大, 只需使损失函数最小。 使用极小值代替最小值:
    $$\frac{\partial}{\partial\theta_{pq}}J(\theta)=-\frac{1}{m}\sum_{t=1}^m\sum_{i=1}^k1\begin{Bmatrix}
        y^t=i
    \end{Bmatrix}\frac{\partial}{\partial\theta_{pq}}=-\frac{1}{m}\sum_{t=1}^m\frac{\partial\ln{\phi_i}}{\partial\theta{pq}}$$
    $$=-\frac{1}{m}\sum_{t=1}^m(1\begin{Bmatrix}
        y^t=p
    \end{Bmatrix}-\phi_p)x_q^t$$
    $$=\frac{1}{m}\sum_{t=1}^m(\phi_p-1\begin{Bmatrix}
        y^t=p
    \end{Bmatrix}x_q^t)$$
    进一步整理为向量式:
    $$\frac{\partial}{\partial\theta_{:j}}J(\theta)=\frac{1}{m}\sum_{i=1}^m\begin{pmatrix}
        \begin{bmatrix}
            \phi_1\\\vdots\\\phi_{k-1}
        \end{bmatrix}-\begin{bmatrix}
            1\begin{Bmatrix}
                y^i=1
            \end{Bmatrix}\\\vdots\\1\begin{Bmatrix}
                1\begin{Bmatrix}
                y^i=k-1
            \end{Bmatrix}
            \end{Bmatrix}
        \end{bmatrix}
    \end{pmatrix}x_j^i$$
    $$=\frac{1}{m}\sum_{i=1}^m\begin{pmatrix}
        h_\theta(x^i)-
        \begin{bmatrix}
            1\begin{Bmatrix}
                y^i=1
            \end{Bmatrix}\\\vdots\\1\begin{Bmatrix}
                y^i=k-1
            \end{Bmatrix}
        \end{bmatrix}
    \end{pmatrix}x_j^i,j=1,\dots,n$$
    使用梯度下降发迭代求解:
    $$\theta_{:j}^{k+1}=\theta_{:j}^k-\partial\frac{\partial}{\partial\theta_{:j}}J(\theta),j=0,1,\dots,n$$


6. 
