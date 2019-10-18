# ソースtarballを作成する

`git archive` コマンドを使う。

```
git archive --format=tar --prefix=prefix-name/ tag | gzip >source-name-tag.tar.gz 
```
