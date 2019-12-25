# untrackedなファイルもdiffに含める

ステージングしていないファイルも `git diff` で差分を見たい場合は `git add -N` で対象のファイルをステージングする。

このとき `git diff HEAD` と実行するとuntrackedなファイルの差分も表示される。
