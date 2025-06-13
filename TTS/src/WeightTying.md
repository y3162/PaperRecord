[戻る](VALL-E.md)
| シンボル | 式 |　意味 |
|:-:|:-:|:-:|
| $c$ | $c\in\mathbb{R}^{C}$ | 入力ベクトル |
| $U$ | $U\in\mathbb{R}^{C\times H}$ | 入力埋め込み |
| $h_{2}$ | $h_{2}=f\left(U^{\top}c\right)$ | 活性化ベクトル<br>$h_{2,i}$ は $U_{i}$ との活性化済み内積(スコア)となる. |
| $V$ | $V\in\mathbb{R}^{C\times H}$ | 出力埋め込み |
| $h_{3}$ | $h_{3}=Vh_{2}$ | スコアベクトル<br>$h_{2,i}$ は $U_{i}$ との内積(スコア)となる. |

言語モデルでは、時刻 $t$ における単語系列 $i_{1:t}$ における、ターゲットの単語(のインデックス) $o_{t}$ の NLL を損失関数とする.
$$
\mathcal{L}_{t}=-\log p_{t}\left(o_{t}\middle|i_{1:t}\right)
$$
この確率は、隠れ状態と出力埋め込みとのスコアによって計算される.
#### $\mathbb{R}^{C}$から確率分布への変換
$$
\begin{align*}
    h_{3}^{\left(t\right)}&=\sum_{i=1}^{H}h_{2,i}^{\left(t\right)}V_{:,i}\\
    p_{t}\left(o_{t}\middle|i_{1:t}\right)&=\frac{\exp\left(h_{3,o_{t}}^{\left(t\right)}\right)}{\sum_{x=1}^{C}\exp\left(h_{3,x}^{\left(t\right)}\right)}
\end{align*}
$$

#### $\mathbb{R}^{H}$から確率分布への変換
$$
\begin{align*}
    p_{t}\left(o_{t}\middle|i_{1:t}\right)&=\frac{\exp\left(V_{o_{t}}^{\top}h_{2}^{\left(t\right)}\right)}{\sum_{x=1}^{C}\exp\left(V_{x}^{\top}h_{2}^{\left(t\right)}\right)}
\end{align*}
$$

#### $U_{k}$の逆伝播
$$
\begin{align*}
    \frac{\partial\mathcal{L}}{\partial U_{k}}&=\frac{\partial\mathcal{L}}{\partial p_{t}}\frac{\partial p_{t}}{\partial h_{2}^{\left(t\right)}}\frac{\partial h_{2}^{\left(t\right)}}{\partial U_{k}}\\
    &=-\frac{1}{p_{t}\left(o_{t}\middle|i_{1:t}\right)}\frac{V_{o_{t}}^{\top}\exp\left(V_{o_{t}}^{\top}h_{2}^{\left(t\right)}\right)\left(\sum_{x=1}^{C}\exp\left(V_{x}^{\top}h_{t}^{\left(t\right)}\right)\right)-\exp\left(V_{o_{t}}^{\top}h_{2}^{\left(t\right)}\right)\sum_{x=1}^{C}V_{x}^{\top}\exp\left(V_{x}^{\top}h_{2}^{\left(t\right)}\right)}{\left(\sum_{x=1}^{C}\exp\left(V_{x}^{\top}h_{t}^{\left(t\right)}\right)\right)^{2}}\frac{\partial h_{2}^{\left(t\right)}}{\partial U_{k}}\\
    &=-\frac{1}{p_{t}\left(o_{t}\middle|i_{1:t}\right)}p\left(o_{t}\middle|i_{1:t}\right)\left(V_{o_{t}}^{\top}-\sum_{x=1}^{C}p\left(x\middle|i_{1:t}\right)V_{x}^{\top}\right)\frac{\partial h_{2}^{\left(t\right)}}{\partial U_{k}}\\
    &=\left(V_{o_{t}}^{\top}-\sum_{x=1}^{C}p\left(x\middle|i_{1:t}\right)V_{x}^{\top}\right)\frac{\partial h_{2}^{\left(t\right)}}{\partial U_{k}}\\
    &=\left(V_{o_{t}}^{\top}-\sum_{x=1}^{C}p\left(x\middle|i_{1:t}\right)V_{x}^{\top}\right)\frac{\partial f}{\partial U_{k}}\left(U^{\top}c\right)
\end{align*}
$$
$c$ が one-hot ベクトルであるときは、$k\neq i_{t}$ での逆伝播が 0 になる.
そうなると出現頻度の低い単語の埋め込みが学習されない.

#### $V_{k}$の逆伝播
$$
\begin{align*}
    \frac{\partial\mathcal{L}}{\partial V_{k}}&=\frac{\partial p_{t}}{\partial V_{k}}\frac{\partial\mathcal{L}_{t}}{\partial p_{t}}\\
    &=\frac{\delta_{o_{t},k}h_{2}^{\left(t\right)}\exp\left(V_{o_{t}}^{\top}h_{2}^{\left(t\right)}\right)\sum_{x=1}^{C}\exp\left(V_{x}^{\top}h_{2}^{\left(t\right)}\right)-\exp\left(V_{o_{t}}^{\top}h_{2}^{\left(t\right)}\right)h_{2}^{\left(t\right)}\exp\left(V_{k}^{\top}h_{2}^{\left(t\right)}\right)}{\left(\sum_{x=1}^{C}\exp\left(V_{x}^{\top}h_{2}^{\left(t\right)}\right)\right)^{2}}\left(-\frac{1}{p_{t}\left(o_{t}\middle|i_{1:t}\right)}\right)\\
    &=\left(p_{t}\left(k\middle|i_{1:t}\right)-\delta_{o_{t},k}\right)h_{2}^{\left(t\right)}
\end{align*}
$$
one-hot ベクトルでも全ての埋め込みが更新される.

$U=V=S$ として重みを共有することで、更新における問題点の解決やや汎化性能の向上が期待される.
