<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [['$','$'], ['\\(','\\)']],
      displayMath: [['$$','$$'], ['\\[','\\]']],
      processEscapes: true,
      processEnvironments: true,
      skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
      TeX: {
        equationNumbers: { autoNumber: "AMS" },
        extensions: ["AMSmath.js"]
      }
    }
  });
</script>

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# ICP的SVD解法不使用拉格朗日乘子法是如何保证旋转矩阵正交性的？

## 1 问题描述

两组点 $\lbrace p_1,p_2,...,p_n\rbrace$ 和 $\lbrace p_1^{'},p_2^{'},...,p_n^{'} \rbrace$ ，求解一个欧式变换$R\in \mathbb{R}^{3\times3}$与平移$t\in \mathbb{R}^{3}$，使得
$$
\min_{R,t}\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-(Rp_i^{'}+t)\parallel^{2}
$$
定义一组点的质心为该组点的算术平均，即
$$
P=\frac{1}{n}\sum_{i=1}^{n}{p_i}\\
P^{'}=\frac{1}{n}\sum_{i=1}^{n}{p_i^{'}}
$$
代入
$$
\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-(Rp_i^{'}+t)\parallel^{2}\\
=\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-Rp_i^{'}-t-(P-RP^{'})+(P-RP^{'})\parallel^{2}\\
=\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-P-R(p_i^{'}-P^{'})+(P-RP^{'}-t)\parallel^{2}\\
=\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-P-R(p_i^{'}-P^{'})\parallel^{2}+2(p_i-P-R(p_i^{'}-P^{'}))^{T}(P-RP^{'}-t)+\parallel P-RP^{'}-t \parallel^{2}\\
$$
其中，第三项$P-RP^{'}-t$不包含$i$，可以移出$\sum$符号外面
$$
\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-(Rp_i^{'}+t)\parallel^{2}\\
=\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-P-R(p_i^{'}-P^{'})\parallel^{2}
+\frac{1}{2}\parallel P-RP^{'}-t \parallel^{2}
+\sum^{n}_{i=1}(p_i-P-R(p_i^{'}-P^{'}))^{T}(P-RP^{'}-t)\\
$$
根据上述质心的定义，第三项中$\sum^{n}_{i=1}(p_i-P-R(p_i^{'}-P^{'}))^{T}=0$，可以化简
$$
\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-(Rp_i^{'}+t)\parallel^{2}\\
=\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-P-R(p_i^{'}-P^{'})\parallel^{2}
+\frac{1}{2}\parallel P-RP^{'}-t \parallel^{2}
$$
第一项仅与旋转矩阵$R$有关，第二项与旋转矩阵$R$和平移矩阵$t$都有关，因此可以先只优化第一项，求得
$$
R^{*}=arg\min_{R}\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-P-R(p_i^{'}-P^{'})\parallel^{2}
$$
之后根据以下公式求得最优的平移矩阵
$$
t^{*} = P-R^{*}P^{'}
$$
记去质心的坐标
$$
q_{i}=p_{i}-P\\
q^{'}_{i}=p_i^{'}-P^{'}
$$
求解问题转化为
$$
R^{*}=arg\min_{R}\frac{1}{2}\sum^{n}_{i=1}\parallel p_i-P-R(p_i^{'}-P^{'})\parallel^{2}\\
=arg\min_{R}\frac{1}{2}\sum^{n}_{i=1}\parallel q_i-Rq_i^{'}\parallel^{2}\\
=arg\min_{R}\frac{1}{2}\sum^{n}_{i=1}q_i^{T}q_i-2q_i^{T}Rq_i^{'}+q_i^{'}R^{T}Rq_i^{'}
$$
旋转矩阵需要满足正交性，即$R^{T}R=I$
$$
R^{*}=arg\min_{R}\frac{1}{2}\sum^{n}_{i=1}q_i^{T}q_i+q_i^{'}q_i^{'}-2q_i^{T}Rq_i^{'}
$$
因为前两项$q_i^{T}q_i+q_i^{'}q_i^{'}$与$R$无关
$$
R^{*}=arg\min_{R}\frac{1}{2}\sum^{n}_{i=1}-2q_i^{T}Rq_i^{'}\\
=arg\max_{R}\sum^{n}_{i=1}q_i^{T}Rq_i^{'}
$$
需要说明的是，$q_i^{T}$是$1\times3$维度的，$R$是$3\times3$维度的$q_i^{'}$是$3\times1$维度的，所以$q_i^{T}Rq_i^{'}$是一个数字，视为$1\times1$矩阵
$$
q_i^{T}Rq_i^{'}=tr(q_i^{T}Rq_i^{'})
$$
矩阵的迹有以下性质
$$
tr(A)=tr(A^T)\\
tr(AB)=tr(BA)\\
tr(ABC)=tr(CAB)=tr(BCA)
$$
所以
$$
q_i^{T}Rq_i^{'}=tr(q_i^{T}Rq_i^{'})=tr(Rq_i^{'}q_i^{T})
$$

$$
\sum^{n}_{i=1}q_i^{T}Rq_i^{'}=\sum^{n}_{i=1}tr(Rq_i^{'}q_i^{T})=tr(R\sum^{n}_{i=1}q_i^{'}q_i^{T})
$$

记
$$
W=\sum^{n}_{i=1}q_i^{'}q_i^{T}\\
$$
那么有
$$
R^{*}=arg\max_{R}\sum^{n}_{i=1}q_i^{T}Rq_i^{'}=arg\max_{R}tr(RW)
$$
这个问题的求解是
$$
W=U\Sigma V^{T}\\

\Sigma=
\left[\begin{matrix}
\sigma_1 & 0 & 0\\
0 & \sigma_2 & 0 \\
0 & 0 & \sigma_3
\end{matrix}\right]\\

R^{*}=arg\max_{R}tr(RW)=VU^{T}
$$
其中，$U,V \in \mathbb{R}^{3\times3}$是正交阵

## 2 相关证明

首先有引理：

对任意半正定矩阵$AA^T$，正交矩阵$B$，都有
$$
tr(AA^T)\ge tr(BAA^T)
$$
记$a_i$为$A$的第$i$列，则
$$
tr(BAA^T)=tr(A^TBA)=\sum a_i^TBa_i
$$
由cauchy-schwarz不等式
$$
a_i^TBa_i=a_i^T(Ba_i)\le \sqrt{(a_i^T a_i)(a_i^TB^TBa_i)}=a_i^T a_i
$$
因此
$$
tr(BAA^T)\le\sum a_i^T a_i=tr(AA^T)
$$
由上述$W$的SVD分解得到
$$
W=U\Sigma V^{T}\\

R^{*}=VU^{T}
$$
可以得到
$$
R^{*}W=VU^{T}U\Sigma V^{T}=V\Sigma V^{T}
$$
这是对称半正定矩阵

对任意的$3\times 3$的正交阵$R$，设$R=BR^{*}$，根据上述引理
$$
tr(R^{*}W)\ge tr(BR^{*}W)=tr(RW)
$$
于是在所有的$3\times 3$的正交阵$R$中，$R^*=VU^T$使得$tr(RW)$最大化

## 3其他证明法

（1）Holder不等式

记$A \in \mathbb{R}^{m\times n} $，$r=min (m,n)$，奇异值从大到小排列$\sigma_1 \ge \sigma_2 \ge ... \ge \sigma_r$

迹范数 $\parallel A \parallel_{tr} = \sum_{i=1}^r \sigma_i$

谱范数$\parallel A \parallel_{sp} = max | \sigma_i |$

有以下不等式
$$
tr(A^TB) \le \parallel A \parallel_{tr}\parallel B \parallel_{sp}
$$
因为$R$是正交阵，奇异值均为1，$\parallel R \parallel_{sp} = 1$
$$
tr(RW)=tr(WR)\le\parallel W \parallel_{tr}\parallel R \parallel_{sp}=\parallel W \parallel_{tr}=tr(\Sigma)
$$
（2）冯诺伊曼不等式

记$A,B \in \mathbb{R}^{n\times n} $，奇异值从大到小排列$\sigma_1 \ge \sigma_2 \ge ... \ge \sigma_n$，$\beta_1 \ge \beta_2 \ge ... \ge \beta_n$

有
$$
tr(AB) \le \sum_{i=1}^n \sigma_i\beta_i
$$
因为$R$是正交阵，奇异值均为1
$$
tr(RW)\le \sum_{i=1}^3 \sigma_i=tr(\Sigma)
$$
运用以上两个不等式可证明
$$
tr(R^*W)=tr(R^*U\Sigma V^T)=tr(\Sigma V^TR^*U)=tr(\Sigma V^T V U^T U)=tr(\Sigma)=max tr(RW) \ge tr(RW)
$$
证毕
