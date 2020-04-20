# 2章の学び
2章で気になった所や感銘した所をまとめています。

## 目次
- [Dockerでのディレクトリの指定法](#Dockerでのディレクトリの指定法)
- [ExcelデータもPandasで扱えます！](#ExcelデータもPandasで扱えます)
- [pivot_tableの新発見引数(aggfunc=size,fill_value=0)](#pivot_tableの新発見引数aggfuncsizefill_value0)
- [要素の総ユニーク数を一瞬で出すunique+lenの合わせ技](#要素の総ユニーク数を一瞬で出すuniquelenの合わせ技)
- [Series.str.upper()でアルファベットを全て小文字→大文字変換する](#Seriesstrupperでアルファベットを全て小文字大文字変換する)
- [Series.str.replace()で全角空白だろうが半角空白だろうが全て消去する！](#Seriesstrreplaceで全角空白だろうが半角空白だろうが全て消去する)
- [DataFrame.sort_values()でデータフレームをソートする](#DataFramesort_valuesでデータフレームをソートする)
- [DFに対してisnull().any()だけでcolumn/indexごとの欠損値の有無が知れる](#DFに対してisnullanyだけでcolumnindexごとの欠損値の有無が知れる)
- [unique&loc&max&~を使ったデータの作成](#uniquelocmaxを使ったデータの作成)
- [min(skipna=false)が示すもの](#minskipnafalseが示すもの)
- [strへ変換後のstr.isdigit()で数値データかどうか判断](#strへ変換後のstrisdigitで数値データかどうか判断)
- [concatでDF同士を繋げる](#concatでDF同士を繋げる)


## 気になったとこ詳細リスト

### Dockerでのディレクトリの指定法
- Dockerを使っているので、working_dirのあれで、指定する構造が深くなってたりする
- 注意なのが`/code/~`としなければいけない。`code/~`ではエラーが出る

```python:jupyter.py
chap2_dir = "/code/sample_code/2章/"
```

### ExcelデータもPandasで扱えます！
- `pd.read_csv`と同様に`pd.read_excel`を使う！

```python:jupyter.py
kokyaku_data = pd.read_excel(chap2_dir + "kokyaku_daicho.xlsx")
kokyaku_data.head()
```

### pivot_tableの新発見引数(aggfunc=size,fill_value=0)
- 個数を出力`aggfunc="size"`と、Nanを0でパディング`fill_value=0`

```python:jupyter.py
res = uriage_data.pivot_table(index="purchase_month", columns="item_name", aggfunc="size", fill_value=0)
```

### 要素の総ユニーク数を一瞬で出すunique+lenの合わせ技
- `pd.uniqueメソッド`でユニークな要素を出力
- `lenメソッド`で長さを取得

```python:jupyter.py
print(pd.unique(uriage_data["item_name"]))
print(len(pd.unique(uriage_data["item_name"])))
```

### Series.str.upper()でアルファベットを全て小文字→大文字変換する
- 元から大文字なのはそのまま。アルファベット小文字は全て大文字へ。

```python:jupyter.py
uriage_data["item_name"] = uriage_data["item_name"].str.upper()
```

### Series.str.replace()で全角空白だろうが半角空白だろうが全て消去する！
- `replace(変換対象, 変換後)`で使う。
- 今回は`" "`と`"　"`を`""`に変換することによって無駄な空白を全て消去

```python:jupyter.py
uriage_data["item_name"] = uriage_data["item_name"].str.replace("　", "")
uriage_data["item_name"] = uriage_data["item_name"].str.replace(" ", "")
```

### DataFrame.sort_values()でデータフレームをソートする
- `by`でソートする対象を指定
- `ascending`で昇順(True)・降順(False)を指定。

```python:jupyter.py
uriage_data.sort_values(by=["item_name"], ascending=True)
```

### DFに対してisnull().any()だけでcolumn/indexごとの欠損値の有無が知れる
- `DataFrame.isnull().any()`のメソッドチェーン的な
- `axis=0`でカラムを指定している

```python:jupyter.py
uriage_data.isnull().any(axis=0)
```

### unique&loc&max&~を使ったデータの作成
1. `True`で値が存在するやつのユニークなリストを作成してfor文
2. `not flg_is_null`なので欠損値があるもので、trgと一致してるか見て、最大値を取得
3. 代入！！！

```python:jupyter.py
for trg in list(uriage_data.loc[flg_is_null, "item_name"].unique()):
    price = uriage_data.loc[(~flg_is_null) & (uriage_data["item_name"] == trg), "item_price"].max()
    uriage_data["item_price"].loc[(flg_is_null) & (uriage_data["item_name"]==trg)] = price
```

### min(skipna=false)が示すもの
- 最小値を出力するが、もしNaNが合った場合、無視するのか否かを決める

### strへ変換後のstr.isdigit()で数値データかどうか判断
1. `astypeメソッド`にてstr型へ変換
2. `str.isdigitメソッド`で数値のみか否かでBool値へ変換
3. `sumメソッド`で`True`の合計数を算出

```python:jupyter.py
flg_is_serial = kokyaku_data["登録日"].astype("str").str.isdigit()
print(flg_is_serial.sum())
```

### concatでDF同士を繋げる

```python:jupyter.py
kokyaku_data["登録日"] = pd.concat([fromSerial, fromString])
```
