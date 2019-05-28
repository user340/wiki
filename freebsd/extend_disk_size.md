# Bhyveの仮想ディスク容量を増やす

`truncate(1)`を使う。

```
# truncate -s +10g disk1.img
```

ファイルシステムの拡張はゲスト側でおこなう。
