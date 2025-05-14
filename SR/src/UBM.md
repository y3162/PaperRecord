[戻る](../list.md)
# まとめ
$Y$を発話、$S$を話者とする.
話者認識のタスクは、以下の仮説を献呈することである.
$$
\begin{align*}
    H_{0}&:Y\text{ is from }S\\
    H_{1}&:Y\text{ is not from }S
\end{align*}
$$
これらの仮説は、対数尤度の比で棄却/採択が決まる.
$$
\frac{p\left(Y\middle|H_{0}\right)}{p\left(Y\middle|H_{1}\right)}
\begin{cases}
    \geq\theta&\quad H_{0}\text{を採択}\\
    <\theta&\quad H_{1}\text{を棄却}
\end{cases}
$$
仮説$H_{0}$のモデルを$\lambda_{\text{hyp}}$で、$H_{1}$のモデルを$\lambda_{\overline{\text{hyp}}}$で表すと、
$$
\Lambda\left(X\right)=\log\,p\left(X\middle|\lambda_{\text{hyp}}\right)-\log\,\left(X\middle|\lambda_{\overline{\text{hyp}}}\right)
$$
が、$\log\,\theta$よりも大きいか小さいかで決めることもできる.

モデル$\lambda_{\text{hyp}}$はうまく定義することができるが、モデル$\lambda_{\overline{\text{hyp}}}$はあまりうまく定義できない.

## 方法1
他の話者のモデルを用いることで、「$S$と異なる話者」であることを表現する.
$$
p\left(X\middle|\lambda_{\overline{\text{hyp}}}\right)=\mathcal{F}\left(p\left(X\middle|\lambda_{1},\dots,p\left(X\middle|\lambda_{N}\right)\right)\right)
$$
良い性能を出すには、$S$と同じバックグラウンドを持つ人から話者$1\dots N$を選ぶ必要がある.

## 方法2
大量の発話データを用いて、モデル$\lambda_{\text{bkg}}$という1つのモデルを作成する.
一度学習すればそのモデルを固定することが利点である.
これがUBMである.
モデル$\lambda_{\text{ubm}}$は、EMアルゴリズムによって最適化される.

## 話者のMAP適応
モデル$\lambda_{\text{ubm}}$からモデル$\lambda_{\text{spk}}$を推定する.[(詳細)](UBM_MAP.md)

モデルが話者性とチャンネル(音響性)とを分離できたとする.
訓練データにない音響環境で録音した際、UBMが持つ訓練データにある音響成分はほぼ0となり、対数尤度比にはほぼ影響しない.
これは、異なる話者であることを判別できるが、同じ話者だと認識できない可能性を孕んでいる.

$\lambda_{\text{ubm}}$と$\lambda_{\text{spk}}$の各成分は対応しているため、$\Lambda$を計算する際は、UBMで値の大きかった$C$個の成分に対してのみ計算するだけで良い.この時計算量は、$2M$から$M+C$となる.
ある音声に対して、$K$人の話者で仮説を検証したい際は、計算量は$\left(K+1\right)M$から$M+KC$となり、$K\gg1$の場合に大幅な計算量削減となる.

## HNORM
録音機器によって、モデル出力のバイアス・ばらつきが異なる.
ハンドセット検出器によってデータをラベル付けする.
音声$X$における話者$S$の対数尤度比を求める際、$S$のモデルの出力を同じラベルを持つ$S$以外の話者(性別は$S$と一致させる)のデータから計算し、以下を計算する.
$$
\Lambda^{\text{HNORM}}\left(X\right)=\frac{\Lambda\left(X\right)-\mu\left(HS\left(X\right)\right)}{\sigma\left(HS\left(X\right)\right)}
$$
話者に依存しない閾値を設定することができる.