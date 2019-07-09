# zsh

## zsh: corrupt history file

```
mv .zsh_histfile .zsh_histfile.bad
strings .zsh_histfile.bad > .zsh_histfile
fc -R .zsh_histfile
```
