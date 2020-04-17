# data-100nock
Python実践データ分析100本ノックの練習リポジトリだよ！

> ダウンロードしてきたCSVファイルや答えは空気を読んでignoreしています！

## １章の学び

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
