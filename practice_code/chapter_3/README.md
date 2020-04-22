# 3章の学び
3章で気になった所や感銘した所をまとめています。

## 目次
- [Dockerでのディレクトリの指定法](#Dockerでのディレクトリの指定法)


## 気になったとこ詳細リスト

### Dockerでのディレクトリの指定法
- Dockerを使っているので、working_dirのあれで、指定する構造が深くなってたりする
- 注意なのが`/code/~`としなければいけない。`code/~`ではエラーが出る

```python:jupyter.py
chap2_dir = "/code/sample_code/2章/"
```