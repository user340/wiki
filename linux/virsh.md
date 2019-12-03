# virshの使い方

## VM一覧の取得

`list` コマンドを使う。

```
$ sudo virsh list
 Id    名前                         状態
----------------------------------------------------
 23     example                        実行中
```

シャットダウンしているVMも一覧に含めたい場合は `--all` オプションをつける。

```
$ sudo virsh list
 Id    名前                         状態
----------------------------------------------------
 23     example                         実行中
  -     centos8                        シャットオフ
```

## VMの起動

`start` コマンドにVM名を渡す。

```
$ sudo virsh start <VM名>
```

## VMのクローン

`clone` コマンドに適宜オプションを渡して実行する。

```
$ sudo virt-clone --original <クローン元> --name <クローン先> --file /var/lib/libvirt/images/<クローン先のディスクイメージ名>
```
