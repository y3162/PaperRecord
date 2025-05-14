[戻る](../list.md)
# まとめ
## JFA
$$
\begin{align*}
    M^{\prime}&=M+c_{\text{corr}}\\
    M&=s+c_{\text{test}}\\
    c&=ux\qquad\text{where}\quad x\sim\mathcal{N}\left(0,I\right)
\end{align*}
$$
| 変数 | 意味 |
|:-:|-|
| $M^{\prime}$ | 観測された話者の特徴量 |
| $M$ | 実際の話者の特徴量 |
| $c$ | チャンネル補正項 |
| $u$ | チャンネル固有行列 |

$c$は$u$の固有ベクトルの線型結合で表されるため、文字通り補正項としてのベクトルと、あるチャンネルの性質を表すベクトルの両方の役割を果たすことができる.

また、$s$は以下のように推定される.
$$
s=m+vy+dz\qquad\text{where}\quad z\sim\mathcal{N}\left(0,I\right)
$$
| 変数 | 意味 |
|:-:|-|
| $m$ | 話者・環境に依存しない特徴量 |
| $v$ | 固有音声行列 |
| $d$ | $v$の対角残差行列 |

## Eigenchannel
Eigenchannleでは、$M$は以下のように表される.
$$
M=m+vy+dz\qquad\text{where}\quad z\sim\mathcal{N}\left(0,I\right)
$$
$M$は、チャンネルの影響を受けやすいが、これをノイズとして扱い、補正項がないことに注意する.