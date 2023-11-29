# Common commands

#### Sessions
```

screen     -- start a screen session
Ctrl-a d   -- detach screen session
screen -r  -- reattach screen session
screen -ls -- list active sessions
screen -S sessionname -X quit -- kill session
```

#### Windows
```

Ctrl-a c      -- create new window
Ctrl-a [0-9]  -- switch to window #
Ctrl-a Ctrl-a -- switch to previous window
Ctrl-a "      -- list windows
Ctrl-a A      -- rename a window
Ctrl-a k      -- kill a window
Ctrl-a :quit  -- kill session
```

#### Visual regions
```

Ctrl-a |   -- vertical split
Ctrl-a S   -- horizontal split
Ctrl-a tab -- switch to next region
Ctrl-a X   -- close a region
```