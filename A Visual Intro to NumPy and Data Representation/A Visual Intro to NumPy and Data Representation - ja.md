# 図解で理解する NumPy とデータ表現

編集についての議論: [Hacker News (366 points, 21 comments)](https://news.ycombinator.com/item?id=20282985), [Reddit r/MachineLearning (256 points, 18 comments)](https://www.reddit.com/r/MachineLearning/comments/c5nc89/p_a_visual_intro_to_numpy_and_data_representation/)  
翻訳一覧: [中国語 1](http://www.junphy.com/wordpress/index.php/2019/10/24/visual-numpy/), [中国語 2](https://github.com/kevingo/blog/blob/master/ML/visual-numpy.md), [日本語](https://note.mu/sayajewels/n/n95edaedb0fc5), [韓国語](https://chloamme.github.io/2021/12/20/visual-numpy-korean.html)

![](https://jalammar.github.io/images/numpy/numpy-array.png)


[NumPy](https://www.numpy.org/)パッケージは、Python上で利用可能なデータ分析、機械学習、科学計算に用いるライブラリです。このライブラリは、ベクトルと行列の計算処理を大幅に簡素化します。いくつかのPythonの主要なパッケージは、計算基盤の基礎部分をNumPyに依存しています。（パッケージの例としてscikit-learn, SciPy, pandas, tensorflowがあります。）数値データを細かく分割する機能に加え、NumPyをマスターすることはこれらのライブラリを用いて高度な状況の処理やデバックをすることが可能になります。

この記事では、いくつかのNumPyの主な利用方法と、機械学習モデルに活用する前段階でさまざまな種類のデータ（表、画像、テキストなど）をどのように表現するかについて解説します。

```
import numpy as np
```

## 配列の作成

NumPy配列（通称：[ndarray](https://docs.scipy.org/doc/numpy/reference/arrays.ndarray.html)）を作成できます。

![](https://jalammar.github.io/images/numpy/create-numpy-array-1.png)

NumPy に配列の値を初期化してもらいたい場合がよくある。NumPy では、このような場合のために `ones()`, `zeros()`, `random.random()` といったメソッドが用意されています。生成してほしい要素数を渡すだけです。

![](https://jalammar.github.io/images/numpy/create-numpy-array-ones-zeros-random.png)

配列の作成が完了したら、次はそれを使って、面白い方法で、操作ができるようになります。

## 配列の算術演算

NumPy の配列の有用性を示すために、2 つの配列を作成してみましょう。 `data`, `ones`と呼ぶことにする。

![](https://jalammar.github.io/images/numpy/numpy-arrays-example-1.png)

位置的に足し算する（各行の値を足す）には、`data + ones` と入力するだけでよい。

![](https://jalammar.github.io/images/numpy/numpy-arrays-adding-1.png)

このようなツールを学び始めたとき、このような抽象化によって、このような計算をループでプログラムする必要がないことが新鮮に感じられたのです。問題をより高いレベルで考えることができる、素晴らしい抽象化です。

![](https://jalammar.github.io/images/numpy/numpy-array-subtract-multiply-divide.png)

配列と一つの数値との演算を行いたい場合がよくある（これをベクトルとスカラとの演算と呼ぶこともある）。例えば、距離を表す配列がマイル単位で、それをキロメートルに変換したいとします。単純に`data * 1.6`と言うだけです。

![](https://jalammar.github.io/images/numpy/numpy-array-broadcast.png)

NumPy がこの操作を、各セルで乗算が起こることを意味すると理解したのがわかるだろうか？その考え方は**ブロードキャスト**と呼ばれ、とても便利なものです。

## インデックス作成

Python のリストをスライスするのと同じように、NumPy の配列をインデックスし、スライスすることができる。

![](https://jalammar.github.io/images/numpy/numpy-array-slice.png)

## アグリゲーション

NumPy が与えてくれるその他の利点は、集約関数である。

![](https://jalammar.github.io/images/numpy/numpy-array-aggregation.png)

`min`、`max`、`sum`に加えて、平均を求める`mean`のような偉大なもの(集約関数)を手に入れることができます。`prod`は、すべての要素を掛け合わせた結果を得ることができます。`std`は、標準偏差を得られ、その他にもたくさんあります。

## より多くの次元で

これまで見てきた例では、すべて 1 次元のベクトルを扱っています。NumPy の美しさの主要な部分は、これまで見てきたすべてを任意の数の次元に適用できることである。

### マトリックスの作成

Python リストを渡して、NumPy にそれを表す行列を作らせることができます。

```
np.array([[1,2],[3,4]])
```

![](https://jalammar.github.io/images/numpy/numpy-array-create-2d.png)

また、上記で紹介した方法(`ones()`, `zeros()`, `random.random()`)と同じように、作成する行列の次元を表すタプルを与えればよい。

![](https://jalammar.github.io/images/numpy/numpy-matrix-ones-zeros-random.png)

### マトリックス算術

2 つの行列が同じ大きさであれば，算術演算子 (`+`, `-`, `\`, `*`, `/`) を用いて行列の加算や乗算を行うことができます。NumPy はこれらを位置演算として扱います。

![](https://jalammar.github.io/images/numpy/numpy-matrix-arithmetic.png)

異なるサイズの行列に対してこれらの算術演算を行うことができるのは、異なる次元が 1 つである場合に限られます。（行列が 1 列または 1 行しかない場合など）この場合、NumPy はその操作にブロードキャストルールを使用する。

![](https://jalammar.github.io/images/numpy/numpy-matrix-broadcast.png)

### ドットプロダクト

算術との違いで重要なのは、ドットプロダクトを使った行列の掛け算の場合です。NumPy では、すべての行列に`dot()`メソッドが用意されており、他の行列との内積演算を行うことができます。

![](https://jalammar.github.io/images/numpy/numpy-matrix-dot-product-1.png)

この図の下に行列の寸法を書き加えているのは、2 つの行列が互いに向き合う側の寸法が同じでなければならないことを強調するためです。この操作は、次のように可視化できます。

![](https://jalammar.github.io/images/numpy/numpy-matrix-dot-product-2.png)

### マトリクスインデックス

インデックスやスライスの操作は、行列を操作するときに、さらに便利になります。

![](https://jalammar.github.io/images/numpy/numpy-matrix-indexing.png)

### マトリクスアグリゲーション

ベクトルを集約するのと同じように、行列を集約することができる。

![](https://jalammar.github.io/images/numpy/numpy-matrix-aggregation-1.png)

行列のすべての値を集約できるだけでなく、axis パラメータを使用すれば、行や列をまたいで集約することも可能です。

![](https://jalammar.github.io/images/numpy/numpy-matrix-aggregation-4.png)

## Transposing and Reshaping

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-transpose.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-reshape.png)

## Yet More Dimensions

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-3d-array.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-3d-array-creation.png)

- 翻訳中

```
array([[[1., 1.],
        [1., 1.],
        [1., 1.]],

       [[1., 1.],
        [1., 1.],
        [1., 1.]],

       [[1., 1.],
        [1., 1.],
        [1., 1.]],

       [[1., 1.],
        [1., 1.],
        [1., 1.]]])
```

## Practical Usage

- 翻訳中

### Formulas

- 翻訳中

![](https://jalammar.github.io/images/numpy/mean-square-error-formula.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-mean-square-error-formula.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-mse-1.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-mse-2.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-mse-3.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-mse-4.png)

- 翻訳中

### Data Representation

- 翻訳中

#### Tables and Spreadsheets

- - 翻訳中

![](https://jalammar.github.io/images/pandas-intro/0%20excel-to-pandas.png)

#### Audio and Timeseries

- - 翻訳中

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-audio.png)

- 翻訳中

#### Images

- - 翻訳中

  - - 翻訳中

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-grayscale-image.png)

- - 翻訳中

  ![](https://jalammar.github.io/images/numpy/numpy-color-image.png)

#### Language

- 翻訳中

- 翻訳中

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-nlp-vocabulary.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-nlp-tokenization.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-nlp-ids.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-nlp-embeddings.png)

- 翻訳中

![](https://jalammar.github.io/images/numpy/numpy-nlp-bert-shape.png)

- 翻訳中

- 翻訳中
