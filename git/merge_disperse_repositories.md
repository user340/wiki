# 別々のリポジトリを一つに統合する

## 別々のリポジトリをリポジトリのあるブランチごとに分ける

1. 統合先のリポジトリを作る（GitHubのWebフロントエンドなどで）
2. 統合先のリポジトリをcloneする（ `git clone example.git` ）
3. 統合対象のリポジトリをremote addする（ `git remote add merging_repo git@github.com:ユーザ名/リポジトリ` ）
4. fetchする（ `git fetch merging_repo` ）
5. 空のブランチをgit checkout --orphanで切ってmergeする（ `git checkout --orphan; git merge merging_repo/master` ）
6. `git push origin ブランチ名` でリモートリポジトリへ反映させる

## 別々のリポジトリをひとつのブランチに統合する

1. 統合先のリポジトリを作る（GitHubのWebフロントエンドなどで）
2. 統合先のリポジトリをcloneする（ `git clone example.git` ）
3. 統合対象のリポジトリをremote addする（ `git remote add merging_repo git@github.com:ユーザ名/リポジトリ` ）
4. fetchする（ `git fetch merging_repo` ）
5. `git merge merging_repo/master` で現在のブランチへマージする

## 後片付け

不要になったリモートブランチは `git remote remove` で削除する。
