[戻る](../list.md)
# 要約

# Glow-TTS
## 定式化
事前分布を $P_{Z}\left(z\middle|c\right)$ として、潜在変数 $z$ は flow デコーダを通して $f_{dec}:z\to x$ に変換される.
メルスペクトログラム $x$ とテキスト系列 $c$ に対して、Glow-TTS は $P_{X}\left(x\middle|c\right)$ を構築する.
$$
\log P_{X}\left(x\middle|c\right)=\log P_{Z}\left(z\middle|c\right)+\log\left|\det\frac{\partial f_{dec}^{-1}\left(x\right)}{\partial x}\right|
$$
$$
P_{Z}\left(z\right)=\mathcal{N}\left(z;\mu,\sigma\right)
$$
事前分布を $\theta$ でパラメータ化し、アライメント関数を $A$ とする.

エンコーダーは $c=c_{1:T_{text}}$ を $\mu=\mu_{1:T_{text}}$ と $\sigma=\sigma_{1:T_{text}}$ へ以下を満たすように写像する.
$$
A\left(j\right)=i\Longrightarrow z_{i}\sim\mathcal{N}\left(z_{j};\mu_{i},\sigma_{i}\right)
$$
このとき、事前分布は以下のように書くことができる
$$
\log P_{Z}\left(z\middle|c;\theta,A\right)=\sum_{j=1}^{T_{mel}}\mathcal{N}\left(z_{j};\mu_{A\left(j\right)},\sigma_{A\left(j\right)}\right)
$$
この設計を満たすような $A,\theta$ を設計したい.
ここでは、設計に対する対数尤度で評価する.
$$
\underset{\theta,A}{\max}\,L\left(\theta,A\right)=\underset{\theta,A}{\max}\,\log P_{X}\left(x\middle|c;\theta,A\right)
$$
$\theta,A$ の両方の探索が難しいため、近似的な開放を用いる.
与えられたパラメータ $\theta$ に対して、
$$
A^{\ast}=\underset{\theta,A}{\arg\max}\,\log P_{X}\left(x\middle|c;A,\theta\right)=\underset{A}{\arg\max}\sum_{j=1}^{T_{mel}}\log\mathcal{N}\left(z_{j};\mu_{A\left(j\right)},\sigma_{A\left(j\right)}\right)
$$
で定まる $A^{\ast}$ をアライメント関数とする.
$$
d_{i}=\sum_{j=1}^{T_{mel}}1_{A^{\ast}\left(j\right)=i}
$$
$$
L_{dur}=MSE\left(f_{dur}\left(sg\left[f_{enc}\left(c\right)\right]\right),d\right)
$$
$sg\left[\cdot\right]$ は勾配停止を表す.
これは、誤ったアライメントにエンコーダーが近づくことを防ぐためである.

## アライメント関数 $A^{\ast}$
$A^{\ast}$ は、動的計画法によって計算される.
$Q_{i,j}$ を $c_{1:i},x_{1:j}$ における対数尤度の最大値とする.
$Q$ の漸化式は次のようになる.
$$
Q_{i,j}=\underset{A}{\max}\sum_{k=1}^{j}\log\mathcal{N}\left(z_{k};\mu_{A\left(k\right)},\sigma_{A\left(k\right)}\right)=\max\left(Q_{i-1,j-1},Q_{i,j-1}\right)+\log\mathcal{N}\left(z_{j};\mu_{i},\sigma_{i}\right)
$$
