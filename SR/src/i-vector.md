[戻る](../list.md)
# まとめ
## JFA
JFA(Joint Factor Analysis)における超ベクトルは以下の式で表される.
$$
M=m+Vy+Ux+Dz
$$
| 変数 | 意味 |
|:-:|-|
| $m$ | 話者・環境に依存しない特徴量 |
| $V$ | 固有音声行列 |
| $U$ | 固有チャンネル行列 |
| $D$ | $V$の対角残差行列 |

## i-vector
一方、i-vectorにおける超ベクトルは以下の式で表される.
$$
M=m+Tw
$$
| 変数 | 意味 |
|:-:|-|
| $m$ | 話者・環境に依存しない特徴量 |
| $T$ | 総変動行列 |
| $w$ | i-vector |

$w$は以下のように推定される.
$$
w=\left(1+T^{t}\Sigma^{-1}N\left(u\right)T\right)^{-1}T^{t}\Sigma^{-1}\tilde{F}\left(u\right)
$$
[(証明)](i-vector_proof.md)

i-vectorは、以下のスコア関数で評価される.
$$
\text{score}\left(w_{\text{target}},w_{\text{test}}\right)=\frac{\left\langle w_{\text{target}},w_{\text{test}}\right\rangle}{\left\|w_{\text{target}}\right\|\left\|w_{\text{test}}\right\|}\gtreqless\theta
$$

## 利点
JFAでは、$y$などを計算するために、特定の話者の登録(enrollment)が必要.
しかし、i-vectorは、大規模なデータセット(UBM)から学習され、特定の話者を登録する必要がない.