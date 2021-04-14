# ZFSミラーリング用にディスクを追加する

1. マシンにディスクを追加する。
2. `dmesg` などでデバイス名を確認する。
3. `zpool attach` でプールにデバイスを追加する。追加後、自動的にミラーリングされる。

Rootをミラーリングする場合はこれより複雑になる。この方法は最後に述べる。

## プールの確認方法

### zpool list -- プールを一覧表示する

```
$ zpool list
NAME     SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
zroot    460G   198G   262G        -         -    36%    43%  1.00x  ONLINE  -
zshare   232G   207G  25.2G        -         -    50%    89%  1.00x  ONLINE  -
```

ここでは システムに`zroot` と `zshare` 2つのプールが存在している。

### zpool status -- プールの状態を確認する

```
$ zpool status
  pool: zroot
 state: ONLINE
status: Some supported features are not enabled on the pool. The pool can
        still be used, but some features are unavailable.
action: Enable all features using 'zpool upgrade'. Once this is done,
        the pool may no longer be accessible by software that does not support
        the features. See zpool-features(7) for details.
  scan: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        zroot       ONLINE       0     0     0
          ada0p4    ONLINE       0     0     0

errors: No known data errors

  pool: zshare
 state: ONLINE
status: One or more devices are configured to use a non-native block size.
        Expect reduced performance.
action: Replace affected devices with devices that support the
        configured block size, or migrate data to a properly configured
        pool.
  scan: resilvered 207G in 0 days 00:18:22 with 0 errors on Sat Apr 10 17:48:03 2021
config:

        NAME        STATE     READ WRITE CKSUM
        zshare      ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            ada1    ONLINE       0     0     0  block size: 512B configured, 4096B native
            ada2    ONLINE       0     0     0  block size: 512B configured, 4096B native

errors: No known data errors

```

`zshare` は `ada1` と `ada2` でミラーリングされている。

実際にデバイスを追加したときは、以下のように実行した。

```
$ zpool attach zshare ada1 ada2
```

`zpool attach` してからミラーリングが完了するには時間がかかる。ミラーリングの経過は `zpool status` で確認できる。

## ZFSのrootをミラーリングする

1. ディスクを追加する。
2. 追加したディスクにGPTを作る。
3. Rootのディスクと同じようにGPTジオメトリを作る。
4. `zpool attach` でプールにパーティションを追加する。追加後、自動的にミラーリングされる。
5. 新しいディスクにブートコードを書き込む。

### 例

rootの `ada0` は次のようなジオメトリになっている。

```
$ gpart show ada0
=>       40  976773088  ada0  GPT  (466G)
         40     409600     1  efi  (200M)
     409640       1024     2  freebsd-boot  (512K)
     410664        984        - free -  (492K)
     411648    4194304     3  freebsd-swap  (2.0G)
    4605952  972167168     4  freebsd-zfs  (464G)
  976773120          8        - free -  (4.0K)
```

`ada3` を追加したとして、同じようにジオメトリを作る。

```
# gpart create -s gpt ada3
# gpart add -t efi -s 200M ada3
# gpart add -t freebsd-boot -s 512k ada3
# gpart add -t freebsd-swap -s 2G -b 411648 ada3
# gpart add -t freebsd-zfs -s 474691M ada3
```

`freebsd-swap` パーティションを作っているとき、
開始セクタを指定して `ada0` と同じく `ada3p2` と `ada3p3` の間に空き領域を作っている点に注意。

また、 `freebsd-zfs` パーティションを作っているときには
`gpart list ada0` で得られた `ada0p4` の `Mediasize` をもとにMB単位で
大きさを指定している点に注意。 `-s 464GB` だとディスク領域が足りないとのエラーが出た。

最後にブートコードを書き込む。

```
# gpart bootcode -b /boot/pmbr -p /boot/gptzfsboot -i 1 ada3
```
