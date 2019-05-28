# セキュアなDockerコンテナを作る

## ユーザを設定する

「絶対に、実働アプリケーションをコンテナ内でrootとして動かしてはいけません。」（Adrian Mouat著, 玉川竜司訳『Docker』, p.320）

任意のユーザグループとユーザを設定し、アプリケーションの実行はこのユーザにやらせる。

```
RUN groupadd -r usergroup && useradd -r -g usergroup user
USER user
```

公式イメージではよくコンテナ内の全ファイルの所有者を特定のユーザに与えている。エントリポイントのスクリプトは以下のようになる。

```
#!/bin/bash
# 『Docker』 p. 321より引用
set -e
if [ "$1" = 'redis-server' ]; then
    chown -R redis .
    exec gosu redis "$@"
fi

exec "$@"
```

## setuid/setgidされたバイナリの削除

Dockerコンテナ内からsetuid/setgidされたバイナリを削除し、権限昇格攻撃を防ぐ。

以下はpython 3.7 stretchコンテナでsetuid/setgidされたバイナリの一覧。

```
$ docker run --rm -it python find / -perm /6000 -type f -exec ls -ld {} \; 2> /dev/null
-rwsr-xr-x 1 root root 61240 Nov 10  2016 /bin/ping
-rwsr-xr-x 1 root root 40536 May 17  2017 /bin/su
-rwsr-xr-x 1 root root 44304 Mar  7  2018 /bin/mount
-rwsr-xr-x 1 root root 31720 Mar  7  2018 /bin/umount
find: ‘/proc/1/task/1/fdinfo/6’: No such file or directory
find: ‘/proc/1/map_files’: Operation not permitted
find: ‘/proc/1/fdinfo/6’: No such file or directory
-rwxr-sr-x 1 root shadow 35592 May 27  2017 /sbin/unix_chkpwd
-rwxr-sr-x 1 root shadow 22808 May 17  2017 /usr/bin/expiry
-rwsr-xr-x 1 root root 40312 May 17  2017 /usr/bin/newgrp
-rwsr-xr-x 1 root root 50040 May 17  2017 /usr/bin/chfn
-rwsr-xr-x 1 root root 59680 May 17  2017 /usr/bin/passwd
-rwxr-sr-x 1 root tty 27448 Mar  7  2018 /usr/bin/wall
-rwsr-xr-x 1 root root 75792 May 17  2017 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 40504 May 17  2017 /usr/bin/chsh
-rwxr-sr-x 1 root shadow 71856 May 17  2017 /usr/bin/chage
-rwxr-sr-x 1 root ssh 358624 Mar  1 16:19 /usr/bin/ssh-agent
-rwsr-xr-x 1 root root 440728 Mar  1 16:19 /usr/lib/openssh/ssh-keysign
```

Dockerfile内でこれらを呼び出す必要がある場合は、Dockerfileの一番最後でこれらを削除するようにする。

```
RUN find / -perm /6000 -type f -exec chmod a-s {} \; || true
```
