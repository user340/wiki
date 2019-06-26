# GNU globalとvimでコードを読みやすくする

## GNU global

ctagsの強化版のようなソフトウェア。devel/global

```
# cd /usr/pkgsrc/devel/global
# make install clean clean-depends
```

### Vimプラグイン

GNU globalのパッケージにvim用のプラグインが用意されている。`~/.vim/plugin`ディレクトリにコピーする。

```
$ mkdir ~/.vim/plugin
$ cp /usr/pkg/share/gtags/gtags.vim ~/.vim/plugin
```

### vimrc

以下を`~/.vimrc`に追記する。

```
nmap <C-q> <C-w><C-w><C-w>q
nmap <C-g> :Gtags -g
nmap <C-l> :Gtags -f %<CR>
nmap <C-j> :Gtags <C-r><C-w><CR>
nmap <C-k> :Gtags -r <C-r><C-w><CR>
nmap <C-n> :cn<CR>
nmap <C-p> :cp<CR>
```

| キーバインド | 説明                                             |
| :--          | :--                                              |
| C-q          | 検索結果のウィンドウを閉じる                     |
| C-g          | コードを`grep `する                              |
| C-l          | 現在開いているファイルの関数一覧を表示する       |
| C-j          | カーソルが置かれている関数・変数の定義元を探す   |
| C-k          | カーソルが置かれている関数・変数の使用箇所を探す |
| C-n          | 次の検索結果へ移動                               |
| C-p          | 前の検索結果へ移動                               |

## 使い方

ソースツリーの最上位ディレクトリで`gtags`を実行する。

```
$ cd src/cvs.NetBSD.org/src
$ gtags
```

以下の3つのファイルはGNU globalで使われるデータベースなので`.cvsignore`や`.gitignore`などに登録しておく。

- GPATH
- GRTAGS
- GTAGS

`gtags`実行後は、vimから上述したキーバインドを用いることで`global`を利用できる。
