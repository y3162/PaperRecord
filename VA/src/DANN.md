[戻る](VPC2020.md)<br>
- $z=E\left(x;\theta_{e}\right)$で潜在表現へ変換.
- $y=D\left(z;\theta_{d}\right)$で元のx-vectorを再構成.
- 攻撃者は、ドメインを特定する.
  - $A_{s}\left(z;\theta_{s}\right)$で話者IDを推定.
  - $A_{g}\left(z;\theta_{g}\right)$で性別を推定.
  - $A_{a}\left(z;\theta_{a}\right)$でアクセントを推定.
再構成における損失は、
$$
\mathcal{L}_{au}\left(\theta_{e},\theta_{d}\right)=\sum_{i=1}^{N}\log P\left(y_{i}\middle|x_{i};\theta_{e},\theta_{d}\right)
$$
攻撃者の識別における損失は、
$$
\mathcal{L}_{z}\left(\theta_{e},\theta_{s},\theta_{g},\theta_{a}\right)=-\sum_{k\in\left\{s,g,a\right\}}\sum_{i=1}^{N}\log P\left(z_{ki}\middle|x_{i};\theta_{e},\theta_{k}\right)
$$
各モデルのパラメータは以下を最適化するように更新される.
$$
\underset{\theta_{e},\theta_{d}}{\min}\underset{\theta_{e},\theta_{s},\theta_{g},\theta_{a}}{\max}\mathcal{L}_{z}\left(\theta_{e},\theta_{s},\theta_{g},\theta_{a}\right)-\lambda\mathcal{L}_{z}\left(\theta_{e},\theta_{s},\theta_{g},\theta_{a}\right)
$$