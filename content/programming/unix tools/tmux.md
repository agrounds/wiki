#### Sessions
```

tmux                       -- create new session
tmux new -s name           -- create new named session
tmux ls                    -- list sessions
tmux attach -t name        -- attach to session
prefix d                   -- detach session
prefix s                   -- list/switch sessions
prefix $                   -- rename session
tmux kill-server           -- kill all sessions (and tmux server)
tmux kill-session -t name  -- kill specific session
```

#### Windows
```

prefix c                -- create window
prefix &                -- kill window (confirm with y)
C-d or exit             -- kill pane/window (no confirmation)
prefix ,                -- rename window
prefix . 2              -- change window number to 2
                           (assumes window #2 currently doesn't exist)
:swap-window -s 1 -t 2  -- swap window #1 and #2
prefix id               -- select window by (number) id
prefix n                -- select next window
prefix p                -- select previous window
prefix l                -- select last window that was viewed (marked with -)
prefix w                -- select window interactively
```

#### Panes
```

prefix %      -- split pane vertically
prefix "      -- split pane horizontally
prefix x      -- close pane (confirm with y)
C-d or exit   -- kill pane/window (no confirmation)
prefix arrow  -- navigate to pane
```

#### Configuration
```

:set -g mouse on/off  -- turn on/off scrolling with mouse
```

#### Other
```

:source-file ~/.tmux.conf  -- reload conf file
```

Inside `.tmux.conf`:
```

set-option -g prefix C-a                   -- set prefix to C-a
set -g default-terminal "screen-256color"  -- explicitly set 256 color support
```

#### Plugins
First install [tmux plugin manager](https://github.com/tmux-plugins/tpm). Then add desired plugins to your `.tmux.conf`:
```shell

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# Other examples:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'github_username/plugin_name#branch'
# set -g @plugin 'git@github.com:user/plugin'
# set -g @plugin 'git@bitbucket.com:user/plugin'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```

Key bindings:
```

prefix I      -- install new plugins and refresh tmux environment
prefix U      -- update plugins
prefix alt-u  -- uninstall plugins that are removed from .tmux.conf
```

#### Resources
- [List of tmux plugins](https://github.com/tmux-plugins/list)
- [Tmux tutorial for beginners](https://dev.to/iggredible/tmux-tutorial-for-beginners-5c52)