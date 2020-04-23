# 4章の学び
4章で気になった所や感銘した所をまとめています。

## 目次
- [遂にきましたScikit-learn!](#遂にきましたScikit-learn)
- [KMeans(教師なし学習)でグルーピング！](#KMeans教師なし学習でグルーピング)
- [クラスタリングした結果をgroupbyで分析](#クラスタリングした結果をgroupbyで分析)
- [次元削除→主成分分析PCA](#次元削除主成分分析PCA)
- [勝手に色分け！？plt.scatter](#勝手に色分け！？plt.scatter)
- [pd.dropna後にreset_index(drop=True)で初期化](#pddropna後にreset_indexdropTrueで初期化)
- [sklearnの線形回帰LinearRegressionでモデル作成](#sklearnの線形回帰LinearRegressionでモデル作成)


## 気になったとこ詳細リスト

### 遂にきましたScikit-learn!
- 言いたかっただけ。笑

### KMeans(教師なし学習)でグルーピング！
- KMeans方でグループ分けするよ！
1. `StandardScaler`はデータを標準化するやつ(データによっての比率や取る値は変わってくるけど、それを標準的に評価してくれるみたいな)
2. `fit_transform`で`customer_clustering`に対して評価前のデータの平均と分散を計算して記憶する。また変換後の渡すやつにも計算を施す
    - ホントは`fit()`と`transform()`で別れているみたい
3. `KMeans`でパラメータを決めてる(4グループ, 乱数制御)
4. `fit`で学習
5. `labels_`で各データをラベルとして出力
6. `unique`で各ラベルを出力

```python:jupyter.py
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler    # ①
sc = StandardScaler()
customer_clustering_sc = sc.fit_transform(customer_clustering)  # ②

kmeans = KMeans(n_clusters=4, random_state=0)
clusters = kmeans.fit(customer_clustering_sc)
customer_clustering["cluster"] = clusters.labels_
print(customer_clustering["cluster"].unique())
customer_clustering.head()
```

### クラスタリングした結果をgroupbyで分析
1. countにてクラスタリングされた人の数を出す

```python:jupyter.py
customer_clustering.columns = ["月内平均値", "月内中央値", "月内最大値", "月内最小値", "会員期間", 'cluster']
customer_clustering.groupby('cluster').count()
```

2. meanにてそれぞれの中央値を算出

```python:jupyter.py
customer_clustering.groupby("cluster").mean()
```

### 次元削除→主成分分析PCA
- 初めて`sklern.decomposition.PCA`を使ってるよw

```python:jupyter.py
from sklearn.decomposition import PCA
X = customer_clustering_sc
pca = PCA(n_components=2)   # 2次元を指定してモデル定義
pca.fit(X)  # ①ここで次元削除
x_pca = pca.transform(X)    # ①ここで次元削除
pca_df = pd.DataFrame(x_pca)
pca_df["cluster"] = customer_clustering["cluster"]  # クラスタリング結果を付与している
```

### 勝手に色分け！？plt.scatter
- 多分for文を回してプロットしていくと、勝手に色分けてくれるのかな？

```python:jupyter.py
import matplotlib.pyplot as plt
%matplotlib inline
for i in customer_clustering["cluster"].unique():
    tmp = pca_df.loc[pca_df["cluster"]==i]
    plt.scatter(tmp[0], tmp[1])
```

### pd.dropna後にreset_index(drop=True)で初期化
- データ内に欠損値がある場合に`dropna`にてindexごと消し去る！
- 全て消し去った後に`reset_index`にてindexを1から振り直す

```python:jupyter.py
predict_data = predict_data.dropna()
predict_data = predict_data.reset_index(drop=True)
predict_data.head()
```

### sklearnの線形回帰LinearRegressionでモデル作成
- パラメータは一切使って無い
- 学習75%,テスト25%でsplitしてる

```python:jupyter.py
from sklearn import linear_model
import sklearn.model_selection
model = linear_model.LinearRegression()
X = predict_data[["count_0", "count_1", "count_2", "count_3", "count_4", "count_5", "period"]]
y = predict_data["count_pred"]
X_train, X_test, y_train, y_test = sklearn.model_selection.train_test_split(X, y)
model.fit(X_train, y_train)
print(model.score(X_train, y_train))
print(model.score(X_test, y_test))
# ↓出力結果
# 0.6138446522066651
# 0.5872140039923792
```
