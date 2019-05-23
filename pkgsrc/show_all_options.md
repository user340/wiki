# ビルドオプションを確認する

`/etc/mk.conf`に`PKG_OPTION.pkgname= xxx`のように書くと、パッケージのビルドオプションを変更できる。

利用可能なオプションを表示するには、パッケージのディレクトリで

```
# make show-all-options
```

と実行する。
