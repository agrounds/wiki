#### Navigation
- `ma` - mark a position with the label `a` (can be any letter)
- `'a` - jump to mark `a`
- `''` - jump back to previous position (e.g. after jumping with `gg`)
- `42g` - go to line 42

#### Searching
- `-i` - case insensitive search
- `/ /pattern/` - search
- `& /pattern/` - only show lines matching `/pattern/` (like `grep`)
- `-G` - turn off search highlighting
- `ESC-u` - clear search highlighting

#### Misc
- `F` - enable `tail -f`-like behavior, Ctrl-C to break out of it
- `-N` - show line numbers
- `-S` - chop long lines, rather than wrapping
- `v` - open file in `$EDITOR`
