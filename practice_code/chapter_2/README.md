# 2章の学び
2章で気になった所や感銘した所をまとめています。

## 目次
- [Dockerでのディレクトリの指定法](#Dockerでのディレクトリの指定法)
- [ExcelデータもPandasで扱えます！](#ExcelデータもPandasで扱えます)
- [pivot_tableの新発見引数(aggfunc=size,fill_value=0)](#pivot_tableの新発見引数aggfunc=sizefill_value=0)
- [要素の総ユニーク数を一瞬で出すunique+lenの合わせ技](#要素の総ユニーク数を一瞬で出すuniquelenの合わせ技)
- [Series.str.upper()でアルファベットを全て小文字→大文字変換する](#Seriesstrupperでアルファベットを全て小文字大文字変換する)
- [Series.str.replace()で全角空白だろうが半角空白だろうが全て消去する！](#Seriesstrreplaceで全角空白だろうが半角空白だろうが全て消去する)
- [DataFrame.sort_values()でデータフレームをソートする](#DataFramesort_valuesでデータフレームをソートする)


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

```python;jupyter.py
uriage_data["item_name"] = uriage_data["item_name"].str.replace("　", "")
uriage_data["item_name"] = uriage_data["item_name"].str.replace(" ", "")
```

### DataFrame.sort_values()でデータフレームをソートする
- `by`でソートする対象を指定
- `ascending`で昇順(True)・降順(False)を指定。

```python;jupyter.py
uriage_data.sort_values(by=["item_name"], ascending=True)
```
