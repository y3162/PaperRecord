[戻る](i-vector.md)<br>
(証明)
$$
M=m+Tw+\epsilon\qquad\text{where}\quad\epsilon\sim\mathcal{N}\left(0,\Sigma\right)
$$
のように、$M$は誤差を含むと考えられる.
$$
\begin{align*}
    N_{c}&=\sum_{t}P\left(c\middle|y_{t},\Omega\right)\\
    \tilde{F}_{c}&=\sum_{t}P\left(c\middle|y_{t},\Omega\right)\cdot\left(y_{t}-m_{c}\right)
\end{align*}
$$
上記を定めると、確率変数$\tilde{F}\left(u\right)$は以下のような分布に従う.
$$
\tilde{F}\left(u\right)|w\sim\mathcal{N}\left(Tw,\Sigma/N\left(u\right)\right)
$$
$w$の最尤推定を行う.
$$
\begin{align*}
    P\left(w\middle|\tilde{F}\left(u\right)\right)&\propto P\left(\tilde{F}\left(u\right)\middle|w\right)P\left(w\right)\\
    \log P\left(w\middle|\tilde{F}\left(u\right)\right)&=\log P\left(\tilde{F}\left(u\right)\middle|w\right)+\log P\left(w\right)+\text{const}\\
    &=\log\mathcal{N}\left(\tilde{F}\left(u\right);Tw,\Sigma/N\left(u\right)\right)+\log\mathcal{N}\left(w;0,I\right)+\text{const}\\
    &=-\frac{1}{2}\left(\tilde{F}\left(u\right)-Tw\right)^{t}\Sigma^{-1}N\left(u\right)\left(\tilde{F}\left(u\right)-Tw\right)-\frac{1}{2}w^{t}w+\text{const}
\end{align*}
$$
左辺の偏微分が0となるような$w$を推定量とする.