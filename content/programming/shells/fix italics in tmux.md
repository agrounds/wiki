I had to take these steps to support displaying italics in a tmux session. It is documented behavior that `screen-*` terms do not support italics.

**Step 1:** Create the following files somewhere on your system

**tmux-256color.terminfo.txt**
```
tmux-256color|tmux with 256 colors,
  sitm=\E[3m, ritm=\E[23m,
  smso=\E[7m, rmso=\E[27m,
  use=screen-256color,
```

**tmux.terminfo.txt**
```
tmux|tmux terminal multiplexer,
  sitm=\E[3m, ritm=\E[23m,
  smso=\E[7m, rmso=\E[27m,
  use=screen,
```

**Step 2:** Run the following commands to install them to `~/.terminfo/some-number/`

```shell
tic -o ~/.terminfo tmux-256color.terminfo.txt
tic -o ~/.terminfo tmux.terminfo.txt
```

**Step 3:** Set the default terminal for client and server in your `.tmux.conf`.
```
set-option -g default-terminal "tmux-256color"
set-option -s default-terminal "tmux-256color"
```
