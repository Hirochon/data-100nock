# １章の学び
１章で気になった所や感銘した所をまとめています。

## 目次
- [pd.concatの引数について(縦か横に結合(ユニオン)するよ！！)](#pd.concatの引数について縦か横に結合ユニオンするよ！！)
- [pd.mergeの引数について(横に主軸を決めてマージするよ！)](#pd.mergeの引数について横に主軸を決めてマージするよ！)
- [pd.mergeすげえ…(これデータへのリテラシー高くないと使えないよねw)](#pd.mergeすげえ…これデータへのリテラシー高くないと使えないよねw)
- [さも当たり前のようにSeries同士の掛け算を実現するPandasさん](#さも当たり前のようにSeries同士の掛け算を実現するPandasさん)
- [sumメソッドでSeriesの合計値だせる！](#sumメソッドでSeriesの合計値だせる！)
- [isnull()使ってPandasで欠損値を見つけに行こう！](#isnull使ってPandasで欠損値を見つけに行こう！)
- [describe()にて数値データのを見いていく！](#describeにて数値データのを見いていく！)
- [日付の最大最小も参照できるよ](#日付の最大最小も参照できるよ)

### pd.concatの引数について(縦か横に結合(ユニオン)するよ！！)
- 下記コードの学びは多い。

    ```python:jupyter.py
    transaction = pd.concat([transaction_1, transaction_2], ignore_index=True, axis=0)
    ```

    1. `pd.cancat`によって、DF同士を結合することができる。
    2. `ignore_index`によってindex番号を振り直している
    3. `axis=0`を指定(縦はしなくても良い)することによって縦に結合することを命令している。
    4. つまり横に結合したい場合は`axis=1`を指定する良い

### pd.mergeの引数について(横に主軸を決めてマージするよ！)
- 下記コードも学びが多いね。

    ```python:jupyter.py
    join_data = pd.merge(transaction_detail, transaction[["transaction_id", "payment_date", "customer_id"]], on="transaction_id", how="left")
    ```

    1. 第１引数にて軸のDFを指定、第２引数にてマージするDFを指定する。
    2. マージするDF(限定？)の`key`(column)は指定することができる
    3. `on`にて主軸にするカラムを指定している。もしユニークであればそれだけindex数は増える。
    4. `how`にてマージ方法を定めている。今回であれば第１引数を左側に？という意味カナ？

### pd.mergeすげえ…(これデータへのリテラシー高くないと使えないよねw)
- 学びになるコードです。

    ```python:jupyter.py
    join_data = pd.merge(join_data, customer_master, on="customer_id", how="left")
    join_data = pd.merge(join_data, item_master, on="item_id", how="left")
    ```

    1. マージする側のindex数がどうであろうと、軸が一致していれば複製して、代入する！！！(しゅごい！！！)

### さも当たり前のようにSeries同士の掛け算を実現するPandasさん

    ```python:jupyter.py
    join_data["price"] = join_data["quantity"] * join_data["item_price"]
    ```

    - これでSeries同士の各index毎の掛け算してます

### sumメソッドでSeriesの合計値だせる！

    ```python:jupyter.py
    print(join_data["price"].sum())
    ```

### isnull()使ってPandasで欠損値を見つけに行こう！

    ```python:jupyter.py
    join_data.isnull().sum()
    ```

    - NaNの合計を出力することができるよ！

### describe()にて数値データのを見いていく！

    ```python:jupyter.py
    join_data.describe()
    ```

    - count: データ件数
    - mean: 平均値
    - std: 標準偏差
    - min: 最小値
    - 25%/50%/75%: 四分位数
    - max: 最大値

### 日付の最大最小も参照できるよ

    ```python:jupyter.py
    print(join_data["payment_date"].min(), join_data["payment_date"].max())
    ```
