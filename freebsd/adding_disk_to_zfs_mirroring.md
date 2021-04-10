# ZFSミラーリング用にディスクを追加する

1. マシンにディスクを追加する。
2. `dmesg` などでデバイス名を確認する。
3. `zpool attach` でプールにデバイスを追加する。追加後、自動的にミラーリングされる。

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
