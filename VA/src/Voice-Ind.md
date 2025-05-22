[戻る](VPC2020.md)<br>
データセット$\mathcal{D}$には、1話者1つのx-vectorが登録されているとする.
今、データセット$\tilde{{\mathcal{D}}}$がデータセット$\mathcal{D}$と$\mathcal{D^{\prime}}$のどちらから作成されたものかが推定されるとする.
ただし、$\mathcal{D}$と$\mathcal{D}^{\prime}$はそれぞれ1つの要素$x,x^{\prime}$が異なっているものとする.
$$
\frac{\Pr\left(\tilde{\mathcal{D}}\middle|\mathcal{D}\right)}{\Pr\left(\tilde{\mathcal{D}}\middle|\mathcal{D}^{\prime}\right)}\leq e^{\epsilon\,d\left(\mathcal{D},\mathcal{D}^{\prime}\right)}
$$
$$
  d\left(\mathcal{D},\mathcal{D}^{\prime}\right)=d_{\chi}\left(x,x^{\prime}\right)
$$
$$
  d_{\chi}\left(x,x^{\prime}\right)=\frac{\arccos\left(\frac{\left\langle x,x^{\prime}\right\rangle}{\left\|x\right\|\left\|x^{\prime}\right\|}\right)}{\pi}
$$
$x$と$x^{\prime}$が大きく異なればデータセットの特徴も大きく異なることを許容することを意味し、このような撹乱アルゴリズム$K$を設計したい.

$x_{0}\in\mathcal{D}$が与えられたときに、匿名化用のx-vectorとして$\tilde{x}\in\mathcal{D}$を決めたい.
このとき、以下の確率に従って選べば良い.
$$
\Pr\left(\tilde{x}\middle|x_{0}\right)\propto e^{-\epsilon\,d_{\chi}\left(x_{0},\tilde{x}\right)}
$$
ただし、$\Pr\left(x_{0}\middle|x_{0}\right)=0$であり、これは匿名化においては元のx-vectorと同じものは選ぶべきではないという考えによるものである.

このとき、先述の条件が満たされていることを確認する.$\tilde{\mathcal{D}}=\left\{\tilde{x}\right\}$として、
$$
\begin{align*}
  \frac{\Pr\left(\tilde{\mathcal{D}}\middle|\mathcal{D}^{\prime}\right)}{\Pr\left(\tilde{\mathcal{D}}\middle|\mathcal{D}\right)}&=\frac{e^{-\epsilon\,d_{\chi}\left(x^{\prime},\tilde{x}\right)}/\sum_{x\in\mathcal{D}^{\prime}\setminus\left\{x^{\prime}\right\}}e^{-\epsilon\,d_{\chi}\left(x,\tilde{{x}}\right)}}{e^{-\epsilon\,d_{\chi}\left(x_{0},\tilde{x}\right)}/\sum_{x\in\mathcal{D}\setminus\left\{x_{0}\right\}}e^{-\epsilon\,d_{\chi}\left(x,\tilde{{x}}\right)}}\\
  &=\frac{e^{-\epsilon\,d_{\chi}\left(x^{\prime},\tilde{x}\right)}}{e^{-\epsilon\,d_{\chi}\left(x_{0},\tilde{x}\right)}}\cdot\frac{\sum_{x\in\mathcal{D}\setminus\left\{x_{0}\right\}}e^{-\epsilon\,d_{\chi}\left(x,\tilde{{x}}\right)}}{\sum_{x\in\mathcal{D}^{\prime}\setminus\left\{x^{\prime}\right\}}e^{-\epsilon\,d_{\chi}\left(x,\tilde{{x}}\right)}}\\
  &=\frac{e^{-\epsilon\,d_{\chi}\left(x^{\prime},\tilde{x}\right)}}{e^{-\epsilon\,d_{\chi}\left(x_{0},\tilde{x}\right)}}\\
  &=e^{-\epsilon\,d_{\chi}\left(x^{\prime},\tilde{x}\right)+\epsilon\,d_{\chi}\left(x_{0},\tilde{x}\right)}\\
  &\leq e^{\epsilon\,d_{\chi}\left(x_{0},x^{\prime}\right)}=e^{\epsilon\,d\left(\mathcal{D},\mathcal{D}^{\prime}\right)}
\end{align*}
$$
第2行は、$\mathcal{D}\setminus\left\{x_{0}\right\}=\mathcal{D}^{\prime}\setminus\left\{x^{\prime}\right\}$による.

第5行は、三角不等式による.[($d_{\chi}$が三角不等式を満たすことの証明)](d_chi.md)
