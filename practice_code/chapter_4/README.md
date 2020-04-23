# 4章の学び
4章で気になった所や感銘した所をまとめています。

## 目次
- [遂にきましたScikit-learn!](#遂にきましたScikit-learn)
- [KMeans(教師なし学習)でグルーピング！](#KMeans教師なし学習でグルーピング)

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
