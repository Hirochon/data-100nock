# 2章の学び
2章で気になった所や感銘した所をまとめています。

## 目次
- [Dockerでのディレクトリの指定法](#Dockerでのディレクトリの指定法)
- [ExcelデータもPandasで扱えます！](#ExcelデータもPandasで扱えます)

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
