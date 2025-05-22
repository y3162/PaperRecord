[戻る](Voice-ind.md)<br>
$$
d_{\chi}\left(x,y\right)=\frac{\arccos\left(\frac{\left\langle x,y\right\rangle}{\left\|x\right\|\left\|y\right\|}\right)}{\pi}
$$
で定まる$d_{\chi}$が三角不等式
$$
    d_{\chi}\left(x,y\right)+d_{\chi}\left(y,z\right)\geq d_{\chi}\left(x,z\right)
$$
を満たすことを示す.以下では、$\left\|x\right\|=\left\|y\right\|=\left\|z\right\|=1$としても一般性を失わない.(0ベクトルがある場合は、簡単に証明できる.)

# 直交変換
$Uy=\left(1,0,\dots,0\right)$となる直交行列$U$を用いると以下が成り立つ.
$$
x^{\prime}=Ux,y^{\prime}=Uy,z^{\prime}=Uz\\
\left\|x^{\prime}\right\|=\left\|y^{\prime}\right\|=\left\|z^{\prime}\right\|=1\\
\left\langle x^{\prime},y^{\prime}\right\rangle=\left\langle x,y\right\rangle\\
\left\langle y^{\prime},z^{\prime}\right\rangle=\left\langle x,y\right\rangle\\
\left\langle x^{\prime},z^{\prime}\right\rangle=\left\langle x,y\right\rangle
$$
従って、$d_{\chi}$を直交変換することができた.
$$
    d_{\chi}\left(x,y\right)=d_{\chi}\left(x^{\prime},y^{\prime}\right)\\
    d_{\chi}\left(y,z\right)=d_{\chi}\left(y^{\prime},z^{\prime}\right)\\
    d_{\chi}\left(x,z\right)=d_{\chi}\left(x^{\prime},z^{\prime}\right)
$$

以下、$\cos\alpha=x_{1}^{\prime},\cos\beta=z_{1}^{\prime},\cos\theta=\left\langle x^{\prime},z^{\prime}\right\rangle$とする.
つまり、$\alpha+\beta\geq\theta$を示したい.
以降、$\alpha+\beta<\pi$のときを考えれば良い.

# 不等式の評価
左辺は、
$$
\begin{align*}
    \left\langle x^{\prime},z^{\prime}\right\rangle&=\sum_{i}x_{i}^{\prime}z_{i}^{\prime}\\
    &=x_{1}^{\prime}z_{1}^{\prime}+\sum_{2\leq i}x_{i}^{\prime}z_{i}^{\prime}\\
    &\geq x_{1}^{\prime}z_{1}^{\prime}-\sqrt{1-{x_{1}^{\prime}}^{2}}\sqrt{1-{z_{1}^{\prime}}^{2}}\\
    &＝\cos\alpha\cos\beta-\sin\alpha\sin\beta\\
    &=\cos\left(\alpha+\beta\right)
\end{align*}
$$
以下を得る.
$$
    \theta=\arccos\left\langle x,z\right\rangle\leq\alpha+\beta
$$
