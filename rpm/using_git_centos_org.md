# git.centos.orgからチェックアウトしたパッケージをビルドする

rpmbuildのルートディレクトリはチェックアウトしたGitリポジトリのディレクトリとする。ホームディレクトリに `rpmbuild` ディレクトリは作らないという想定。

## 必要なパッケージ

- rpmdevtools
- rpm-build

## チェックアウトする

`git checkout` で取得するブランチ名は https://git.centos.org/rpms から確認する。

```
$ git clone https://git.centos.org/rpms/XXX.git
$ cd XXX/
$ git checkout -b c8 origin/c8
```

## spectoolでソースコードを取得する

```
spectool -g -R SPECS/XXX.spec -C SOURCES
```

## rpmbuildでビルドする

```
rpmbuild -ba --define "%_topdir $PWD" SPECS/XXX.spec
```
