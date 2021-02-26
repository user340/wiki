# Patch

## パッチを作る

### `diff` で作るやり方

1. オリジナルを退避させる
2. ファイルを編集する
3. `diff -u` で差分をとり `.patch` ファイルに書き出す

```
$ cp example.c example.c.orig       # オリジナルを作る
$ vim example.c                     # ファイルを編集する
...
$ diff -u example.c.orig example.c > example.patch # 差分を取る
```

### `git diff` で作るやり方

任意のGitリポジトリで変更を加えたあと、 `git diff` する。

このコマンドで出力したパッチは `patch` コマンドでも使える。

## パッチを適用する

```
$ patch < xxx.patch
```

## パッチの変更分を戻す

```
$ patch -R < xxx.patch
```
