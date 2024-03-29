[参考](https://tutorials.chainer.org/ja/13_Basics_of_Neural_Networks.html)

## 13.2 ニューラルネットワークとは
ニューラルネットワークは、微分可能な変換を繋げて作られた計算グラフ (computational graph) 
### 13.2.1 layer
層と層の間にあるノード間の結合は、一つ一つが重みを持っており、全結合型ニューラルネットワークの場合は、それらの重みをまとめて、一つの行列で表現します。
**この際、単回帰分析と重回帰分析 の章で解説した重回帰分析の例と同じように、バイアスをその重み行列に含めて扱うため、入力層の最後に常に 1 を持つノードが追加されていることに注意してください。**
→ あとでやる！  
連続的な実数値を予測する回帰問題の場合は、目標値の種類に合わせて出力層のノード数を決定します。
### 13.2.2 architecture
**ハイパーパラメータ** 自分で決定するパラメータ ex) 中間層のノード数
### 13.2.3 type
- 層間のノードが互いに密に結合した全結合型 (fully-connected)
- 画像処理などでよく用いられる畳み込み型 (convolutional)
- 系列データの扱いによく用いられる再帰型 (recurrent)   
→ 別々の層間には別々の種類の結合の仕方を用いることができる

## 13.3 ニューラルネットワークの計算
### 13.3.1. 基本的な計算の流れ
**順伝播 (forward propagation)** 入力が与えられたとき、ニューラルネットワークの各層を順番に計算していき、出力まで計算を行うこと
ニューラルネットワークの各層では、前の層の出力値に「線形変換」と「非線形変換」を順番に施している
### 13.3.2. 線形変換
全結合型ニューラルネットワークでは、層と層の間で線形変換が行われている
ニューラルネットワークの隠れ層では、一つ前の層に線形変換を適用した結果を受け取り、そこへさらに非線形変換を適用したものを出力する
### 13.3.3 非線形変換
**活性化関数 (activation function)**
**活性値（activation）**
線形変換を行った結果に活性化関数を用いて非線形変換を行なった結果
#### 13.3.3.1. ロジスティックシグモイド関数
h = a(u) = 1/(1 + exp(-u))
→ あまり使われない
**勾配消失 (vanishing gradient)**によって学習が進行しなくなる問題が発生しやすくなるため
#### 13.3.3.2. ReLU
h = a(u) = max(0, u)  
**正規化線形関数 (ReLU: rectified linear unit)** 
ReLU は入力が負の値の場合には出力は 0 で一定であり、正の値の場合は入力をそのまま出力するという関数
### 13.3.4. 数値を見ながら計算の流れを確認
→ あとでやる
## 13.4. ニューラルネットワークの訓練
**解析的に解く** 変数のままで解を求めること  
**数値的に解く** 計算機を使って繰り返し数値計算を行って解を求める
ニューラルネットワークでは、基本的に数値的な手法によって最適なパラメータを求める
求めたいパラメータの初期値 (initial value) を事前に決めておく必要がある  
**最適化手法** 1. モデルを設計し 2. 目的関数を定め 3. 目的関数を最適化するパラメータを求める
### 13.4.1. 目的関数
ニューラルネットワークの訓練には、微分可能でさえあれば解きたいタスクに合わせて様々な目的関数を利用することができる
- 回帰問題でよく用いられる平均二乗誤差 (mean squared error)
- 分類問題でよく用いられる交差エントロピー (cross entropy)
#### 13.4.1.1. 平均二乗誤差
**平均二乗誤差 (mean squared error)** 回帰問題を解きたい場合によく用いられる目的関数
#### 13.4.1.2. 交差エントロピー
**交差エントロピー (cross entropy)** 分類問題を解きたい際によく用いられる目的関数
**ワンホットベクトル（1-hot vector）** 𝑘 (𝑘=1,2,…,𝐾)  のいずれか 1 つだけが 1 であり、それ以外は 0 であるようなベクトル  
### 13.4.2. 目的関数の最適化
目的関数の値を最小にするようなパラメータの値を求めることで、ニューラルネットワークを訓練します
最適化アルゴリズムの一つである勾配降下法 (gradient descent) 
#### 13.4.2.1. 勾配降下法
ニューラルネットワークのパラメータは乱数で初期化されます
→ あとでやる
### 13.4.3. ミニバッチ学習
**ミニバッチ学習** いくつかのデータをまとめて入力し、それぞれの勾配を計算したあと、その勾配の平均値を用いてパラメータの更新を行う
1. 訓練データセットから一様ランダムに Nb (>0) 個のデータを抽出する
2. その Nb 個のデータをまとめてニューラルネットワークに入力し、それぞれのデータに対する目的関数の値を計算する
3. Nb 個の目的関数の値の平均をとる
4. この平均の値に対する各パラメータの勾配を求める
5. 求めた勾配を使ってパラメータを更新する
**バッチサイズ (batch size)** 一度のパラメータ更新に用いられるサンプルの数Nb
**1 イテレーション (iteration)** 勾配降下法において、データを用いてパラメータを1度更新するまでのこと
**1 エポック = 1 イテレーション** バッチ処理ではこうなる！
**確率的勾配降下法（stocastic gradient descent; SGD）** ミニバッチ学習を用いた勾配降下法
### 13.4.4. パラメータ更新量の算出
