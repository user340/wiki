# ブランチ名を変更する

## 現在のブランチ名を変更する

```
$ git branch -m <NewBranchName>
```

## 指定のブランチ名を変更する

```
$ git branch -m <OldBranchName> <NewBranchName>
```

## リモートへpushする

古いブランチを削除したあと、新しいブランチでリモートへpushする。

```
$ git push --delete origin <OldBranchName>
$ git push origin <NewBranchName>
```
