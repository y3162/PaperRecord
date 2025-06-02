[戻る](../list.md)
# 第2章
## 2.2
### フレーム処理
| 記号 | 意味 |
|:-:|:-:|
| $s$ | 信号 |
| $w$ | 窓関数 |
| $N$ | フレーム長 |
| $T$ | フレーム間隔 |
$$
s_{W}\left(m;l\right)=w\left(m\right)s\left(l+m\right)\qquad\left(l=0,T,2T,\cdots\right)
$$
窓関数は以下の2つが主流である.
$$
\begin{align*}
    \text{ハミング窓} && w\left(n\right)&=0.54-0.46\cos\left(\frac{2n\pi}{N-1}\right)&&\left(n=0,\cdots,N-1\right)\\
    \text{ハニング窓} && w\left(n\right)&=0.5-0.5\cos\left(\frac{2n\pi}{N-1}\right)&&\left(n=0,\cdots,N-1\right)
\end{align*}
$$
窓関数は、フーリエ変換における周期性の仮定による、高周波数成分の発生を抑える効果がある.
$N=2T\sim3T$とすることが多い.

### 離散時間フーリエ変換(DTFT)
$$
S\left(e^{j\omega}\right)=\sum_{n=0}^{N-1}s_{W}\left(n\right)e^{-j\omega n}
$$
通常これは、高速フーリエ変換によって以下のような周波数に限定して計算される.
$$
S^{\prime}\left(k\right)=S\left(e^{j\frac{2\pi k}{N}k}\right)=\sum_{n=0}^{N-1}s_{W}\left(n\right)e^{-j\frac{2\pi k}{N}k}\qquad\left(k=0,\cdots,N-1\right)
$$

### 線形予測分析(LPC分析)
長さ $N$ の信号 $s_{W}\left(0\right),\cdots,s_{W}\left(N-1\right)$ が与えられたとして、
$$
\hat{s}_{W}\left(n\right)=\sum_{i=1}^{p}a_{i}s_{W}\left(n-i\right)
$$
で、線形回帰することを考える.
各フレームでは周期性が仮定されていることに注意したい.
このとき、誤差は以下で与えられる.
$$
e\left(n\right)=s_{W}\left(m\right)-\hat{s}_{W}\left(m\right)
$$
この誤差信号のパワーを最小化するような係数 $a_{i}$ を採用する.

伝達特性は、
$$
\begin{align*}
    S_{W}\left(n\right)&=\sum_{i=1}^{p}a_{i}z^{-i}S_{W}\left(n\right)+E\left(n\right)\\
    \left\{1-\sum_{i=1}^{p}a_{i}z^{-i}\right\}S_{W}\left(n\right)&=E\left(n\right)\\
    \frac{S_{W}\left(n\right)}{E\left(n\right)}&=\frac{1}{1-\sum_{i=1}^{p}a_{i}z^{-i}}
\end{align*}
$$
これにより、LPC分析によるパワースペクトルは以下で与えられる.
$$
\left|H\left(e^{j\omega}\right)\right|^{2}=\frac{1}{\left|1-\sum_{i=1}^{p}a_{i}e^{-ij\omega}\right|^{2}}
$$

### ケプストラム分析
音声信号は、音源信号が声道の伝達特性に畳み込まれたものであり、分離したい.
声源の方が声道よりも周波数的に滑らかであることを利用して分離を試みる.

音声信号 $s$ に対して、音源信号を $g,$ 声道の畳み込み関数を $h$ とすると、$s=g\ast h$ であり、
$$
\begin{align*}
    \left|S\left(e^{jw}\right)\right|&=\left|G\left(e^{jw}\right)\right|\cdot\left|H\left(jw\right)\right|\\
    \log\left|S\left(e^{jw}\right)\right|&=\log\left|G\left(e^{jw}\right)\right|+\log\left|H\left(jw\right)\right|
\end{align*}
$$
これを逆フーリエ変換すると(ケプストラム係数)、低次のケフレンシーには $H$ の特性が、高次のケフレンシーには $G$ の特性が現れるはずである.

## 2.3
### MFCCパラメータ
$$
m\left(l\right)=\sum_{k=lo}^{hi}W\left(k;l\right)\left|S^{\prime}\left(k\right)\right|\qquad\left(l=1,\cdots,L\right)\\
W\left(k;l\right)=
\begin{cases}
    \frac{k-k_{lo}\left(l\right)}{k_{c}\left(l\right)-k_{lo}\left(l\right)} & \left(k_{lo}\left(l\right)\leq k\leq k_{c}\left(l\right)\right)\\
    \frac{k_{hi}\left(l\right)-k}{k_{hi}\left(l\right)-k_{c}\left(l\right)} & \left(k_{c}\left(l\right)\leq k\leq k_{hi}\left(l\right)\right)
\end{cases}
$$
$$
k_{c}\left(l\right)=k_{hi}\left(l-1\right)=k_{lo}\left(l+1\right)
$$
$k_{c}\left(l\right)$ はメル周波数軸上で等間隔に配置される.

周波数 $f\left[\text{Hz}\right]$ はメル周波数 $\operatorname{Mel}\left(l\right)=2595\log_{10}\left(1+\frac{f}{700}\right)$ に対応する.

$m\left(l\right)$ を離散コサイン変換することで、MFCC が求まる.
$$
c_{\text{mfcc}}\left(i\right)=\sum_{l=1}^{L}\log m\left(l\right)\,\cos\left\{\left(l-\frac{1}{2}\right)\frac{i\pi}{L}\right\}
$$
