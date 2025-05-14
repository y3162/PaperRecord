[戻る](UBM.md)<br>
UBMモデルは、以下のように与えられる.
$$
\begin{align*}
    p\left(x\middle|\lambda\right)&=\sum_{i=1}^{M}w_{i}\,\mathcal{N}\left(x;\mu_{i},\Sigma_{i}\right)\\
    \log\,p\left(X\middle|\lambda\right)&=\sum_{t=1}^{T}\log\,p\left(x_{t}\middle|\lambda\right)
\end{align*}
$$
ここで、共分散行列は対角行列であるとする.

トレーニングデータにおける混合成分$i$の(推定)個数は、
$$
\begin{align*}
    n_{i}&=\sum_{t=1}^{T}\Pr\left(i\middle|x_{t}\right)\\
    \Pr\left(i\middle|x_{t}\right)&=\frac{w_{i}p_{i}\left(x_{t}\right)}{\sum_{j=1}^{M}w_{j}p_{j}\left(x_{t}\right)}
\end{align*}
$$
これに対し、成分$i$に属する(と推定された)データ各統計量は以下のように定めることが出来る.

$$
\begin{align*}
    \mathbb{E}\left[x\right]&=\frac{1}{n_{i}}\sum_{t=1}^{T}\Pr\left(i\middle|x_{t}\right)x_{t}\\
    \mathbb{E}\left[x^{2}\right]&=\frac{1}{n_{i}}\sum_{t=1}^{T}\Pr\left(i\middle|x_{t}\right)x_{t}^{2}
\end{align*}
$$
$x_{t}^{2}$は、各要素を2乗したベクトルである.

そして、現時点でのパラメータ$\rho$の信用度を$r^{\rho}$として、パラメータ$\rho$の減衰率を以下で定める.
$$
\alpha_{i}^{\rho}=\frac{n_{i}}{n_{i}+r^{\rho}}
$$
更新式は以下のようになる.
$$
\begin{align*}
    \hat{w}_{i}&=\left[\alpha_{i}^{w}\left(\frac{n_{i}}{T}\right)+\left(1-\alpha_{i}^{w}\right)w_{i}\right]\gamma\\
    \hat{\mu}_{i}&=\alpha_{i}^{m}\mathbb{E}\left[x\right]+\left(1-\alpha_{i}^{m}\right)\mu_{i}\\
    \hat{\sigma}_{i}&=\alpha_{i}^{v}\mathbb{E}\left[x^{2}\right]+\left(1-\alpha_{i}^{v}\right)\left(\sigma_{i}^{2}+\mu_{i}^{2}\right)-\hat{\mu}_{i}^{2}
\end{align*}
$$
という指数移動平均でパラメータを更新する.
$\gamma$は、$w_{i}$を正規化するための変数で、
$$
\gamma=\frac{1}{\sum_{i=1}^{M}\left[\alpha_{i}^{w}\left(\frac{n_{i}}{T}\right)+\left(1-\alpha_{i}^{w}\right)w_{i}\right]
}$$
通常のMAP適応では、
$$
\hat{w}_{i}=\frac{n_{i}+r^{w}}{T+Mr^{w}}
$$
とするが、経験的に前者の方がパフォーマンスが良いことがわかっている.

### $\alpha_{i}^{\rho}$について
$n_{i}\ll T$のとき、$\alpha_{i}^{\rho}\approx0$であり、混合成分$i$はUBMの情報を維持する.一方、$1\ll n_{i}$のときは、$\alpha_{i}^{\rho}\approx1$であり、話者のデータに適応される.
これによって、少ないデータに対しても堅牢なモデルとなる.

$r^{\rho}$を調整することで、各パラメータの減衰率を調整することが出来る.
しかし、これを調整することによるパフォーマンスの向上はわずかであり、$r^{\rho}=r$で一定とする.
ただし、特定のパラメータのみを更新したいときは、他のパラメータの減衰率を$0$とすることがある.