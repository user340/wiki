# depotパッケージをインストールする

## プロダクト名の確認

```
# swlist -s /path/to/package.depot
```

depotパッケージは絶対パスで指定しなければならない。

## インストール

```
# swinstall -s /path/to/package.depot XXX
```

ここで、`XXX` は`swlist` で出力されたプロダクト名を指定する。

## アンインストール

```
# swremove XXX
```

プロダクト名を指定する。
