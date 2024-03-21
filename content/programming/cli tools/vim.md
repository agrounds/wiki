# .vimrc

### Minimum settings

```vim
set tabstop=2 shiftwidth=2 expandtab  " or 4 to use 4 spaces instead
syntax on
filetype indent on
set smartindent
set backspace=2  " allow backspacing over everything in insert mode
```

### Optional settings

```vim
set tags+=~/.tags  " look for tags in the file ~/.tags
set background=dark  " fix colors (e.g. when using tmux or screen)
```

# Cheatsheet

| command | description |
| ---- | ---- |
| `"*y` | yank selection into clipboard |
| `gg=G` | fix indentation for entire file |
| `''` | jump back to where you were (not sure how this is determined) |
| `:set syntax=json`<br>`:set filetype=json` | Use the specified language highlighting and formatting, respectively, in the current session.  |
