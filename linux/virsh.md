# virshの使い方

## Red Hatによるドキュメント

https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/5/html/virtualization/chap-virtualization-managing_guests_with_virsh

## 早見表

| 目的               | コマンド                |
| :--                | :--                     |
| VMの一覧を表示する | `virsh list`            |
| VMを起動する       | `virsh start <VM名>`    |
| VMを削除する       | `virsh undefine <VM名>` |
| VMの構成を編集する | `virsh edit <VM名`      |

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

## VMの削除

`undefine` コマンドにVM名を渡す。

```
$ sudo virsh undefine <VM名>
```

ストレージごと削除する場合は `--remove-all-storage` オプションも一緒に渡す。

```
$ sudo virsh undefine --remove-all-storage <VM名>
```

## VMの編集

`edit` コマンドにVM名を渡す。

```
$ sudo virsh edit <VM名>
```
