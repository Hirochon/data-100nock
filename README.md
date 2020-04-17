# data-100nock
Python実践データ分析100本ノックの練習リポジトリだよ！

> ダウンロードしてきたCSVファイルや答えは空気を読んでignoreしています！

## １章の学び

### concatの引数について
    - 下記コードの学びは多い。
    ```python:jupyter.py
    transaction = pd.concat([transaction_1, transaction_2], ignore_index=True, axis=0)
    ```
    1. `pd.cancat`によって、DF同士を結合することができる。
    2. `ignore_index`によってindex番号を振り直している
    3. `axis=0`を指定(縦はしなくても良い)することによって縦に結合することを命令している。
    4. つまり横に結合したい場合は`axis=1`を指定する良い
