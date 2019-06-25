# C言語のlanguage serverを使う

## GNU/Linux

### LLVMバイナリをインストールする。

http://releases.llvm.org/download.html からPre-Built Binariesをダウンロードし、展開して各ディレクトリを `/usr/local` へコピーする。

## vimのlanguage-serverプラグインをインストールする

```
Plugin 'prabirshrestha/asyncomplete.vim'
Plugin 'prabirshrestha/async.vim'
Plugin 'prabirshrestha/vim-lsp'
Plugin 'prabirshrestha/asyncomplete-lsp.vim'
Plugin 'prabirshrestha/asyncomplete-buffer.vim'
Plugin 'yami-beta/asyncomplete-omni.vim'
if executable('clangd')
    au User lsp_setup call lsp#register_server({
        \ 'name': 'clangd',
        \ 'cmd': {server_info->['clangd']},
        \ 'whitelist': ['c', 'cpp'],
        \ })
endif
let g:lsp_diagnostics_enabled = 0
```
