# git pullができないとき

```
$ git pull
error: cannot open .git/FETCH_HEAD: No such file or directory
```

`.git/FETCH_HEAD`の権限が適切ではない。`chown`や`chmod`などで権限を変更する。
