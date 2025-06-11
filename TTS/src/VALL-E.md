[戻る](../list.md)
# まとめ
- 強力なコンテキスト内学習の能力を持つ VALL-E を提案する.これにより、ファインチューニング等を用いないゼロショット TTS が可能となる.
- 話者レベルで汎化された TTS の実現により、TTS における半教師ありデータの過小評価を訴えた.
- 同じ話者とテキストに対して、音響環境と表情を変えないまま、多様な音声を生成することに成功した.
- LibriSpeech と VCTK でのゼロショット TTS として SOTA.

## VALL-E
### 定式化
| シンボル | 式 | 意味 |
|:-:|:-:|:-:|
| $\mathcal{D}$ | $\mathcal{D}=\left\{\mathbf{x}_{i},\mathbf{y}_{i}\right\}$ | データセット |
| $\mathbf{x}$ | $\mathbf{x}=\left\{x_{0},\dots,x_{L}\right\}$ | 発音記号系列 |
| $\mathbf{y}$ |  | 音声サンプル |
| $\text{Encodec}\left(\cdot\right)$ | $\text{Encodec}\left(\mathbf{y}\right)=\mathbf{C}$ | エンコーダー(事前学習済み) |
| $\mathbf{C}$ | $\mathbf{C}\in \left\{0,1\right\}^{\left(T+1\right)\times8}$ | 音響プロンプト行列 |
| $T$ |  | ダウンサンプリングした後の系列長 |
| $\mathbf{c}_{t,:}$ |  | 時刻 $t$ における 8 ビット長のコード |
| $\mathbf{c}_{:,j}$ |  | $j$ 番目のコードブック系列 |
| $\text{Decodec}\left(\cdot\right)$ | $\text{Decodec}\left(\mathbf{c}\right)=\hat{\mathbf{y}}$ | デコーダー(事前学習済み) |

ある話者に対するデータを $\left\{\mathbf{x},\mathbf{y}\right\},\left\{\tilde{\mathbf{x}},\tilde{\mathbf{y}}\right\}$ とする. $\mathbf{C},\tilde{\mathbf{C}}$ をそれぞれの音声に対する音響プロンプトとして、
$$
p\left(\mathbf{C}\middle|\mathbf{x},\tilde{\mathbf{C}}\right)
$$
を最大にすることが目的である.
音響プロンプトから、その話者と与えられた発話内容に対応する音響プロンプトを作成することを意味する.

### 学習
$$
\begin{align*}
    p\left(\mathbf{c}_{:,1}\middle|\mathbf{x},\tilde{\mathbf{c}}_{:,1};\theta_{AR}\right)&=\prod_{t=0}^{T}p\left(\mathbf{c}_{t,1}\middle|\mathbf{c}_{<t,1},\tilde{\mathbf{c}}_{:,1},\mathbf{x};\theta_{AR}\right)\\
    p\left(\mathbf{c}_{:,2:8}\middle|\mathbf{x},\tilde{c};\theta_{NAR}\right)&=\prod_{j=2}^{8}p\left(\mathbf{c}_{:,j}\middle|\mathbf{c}_{:,<j},\mathbf{x},\tilde{\mathbf{C}};\theta_{NAR}\right)\\
    p\left(\mathbf{C}\middle|\mathbf{x},\tilde{\mathbf{C}};\theta\right)&=p\left(\mathbf{c}_{:,1}\middle|\mathbf{x},\tilde{\mathbf{c}}_{:,1};\theta_{AR}\right)\prod_{j=2}^{8}p\left(\mathbf{c}_{:,j}\middle|\mathbf{c}_{:,<j},\mathbf{x},\tilde{\mathbf{C}};\theta_{NAR}\right)
\end{align*}
$$

#### AR モデル
入力 $\mathbf{x},\mathbf{c}_{:,1}$ の最後には $\text{<EOS>}$ が挿入され、それぞれに sin 波の位置埋め込みが施される.
また、投影層の埋め込み行列 $W_{a}$ は共有されている.

#### NAR モデル
$$
\begin{align*}
    e_{\mathbf{c}_{t,j}}&=W_{a}^{j}\odot c_{t,j}\\
    \mathbf{e_{c_{t<i}}}&=\sum_{j=1}^{i-1}e_{c_{t,j}}\\
    \mathbf{e_{\tilde{c}_{t}}}&=\sum_{j=1}^{8}e_{\tilde{c}_{t,j}}\\
    \mathbf{e}_{\mathbf{x}}&=\sum_{t=0}^{T}e_{x_{t}}
\end{align*}
$$
$\odot$ はインデックス選択を表している.
つまり、$W_{a}^{j}$ に $j$ 番目に対応する埋め込み表現が格納されており、$c_{t,j}$ がそれを取り出すかどうかの真理値を表していると考えられる.

transformer には、$\left(\mathbf{e_{x}},\mathbf{e}_{\tilde{\mathbf{c}}},\mathbf{e}_{\mathbf{c}_{:,<i}}\right)$ が入力として与えられる.
それぞれに位置埋め込みが施されている.

第 $i$ 層における中間表現 $h$ は AdaLN で変換される.
$$
\mathrm{AdaLN}\left(h,i\right)=a_{i}\mathrm{LayerNorm}\left(h\right)+b_{i}
$$
$a_{i},b_{i}$ はステージ埋め込みの線形変換によって得られる.
$j$ 層目における出力層と $j+1$ 層目における埋め込み層での埋め込み $W_{a}^{j}$ は共有されている.
つまり、
$$
W_{a}^{j}=\begin{pmatrix}
{\mathbf{w}_{j,0}^{\top}} \\ \mathbf{w}_{j,1}^{\top} \\
\end{pmatrix}
$$
に対して、$j$ 層目の最終出力が $\mathbf{z}$ だったとき
$$
c_{t,j}=f\left(W_{a}^{j}\mathbf{z}\right)=f\left(\mathbf{w}_{j,0}^{\top}\mathbf{z}+\mathbf{w}_{j,1}^{\top}\mathbf{z}\right)
$$
で次のトークンの予測をして、
$$
e_{t,j+1}=W_{a}^{j+1}\odot c_{t,j+1}
$$
により埋め込み表現を獲得するが、$W_{a}^{j}=W_{a}^{j+1}$ としているということである.
