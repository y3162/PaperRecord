[戻る](../list.md)
# まとめ
制限付きボルツマンマシン(RBM)は、二値ユニットを使用している.
徐々に小さくなるバイアスと、共有された重みを持つ無限のユニットを用いたところ、NORBやWildなどのデータセットで性能が向上した.
$$
\sum_{i=1}^{\infty}\sigma\left(x-i+0.5\right)
$$
これは以下の式で近似可能である.
$$
\text{ReLU}\left(x\right)\coloneqq\max\left(0,x\right)
$$
連続値ユニットを用いるモデルでReLUを用いると良い性能が得られる可能性がある.