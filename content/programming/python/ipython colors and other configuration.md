Starting with IPython version 8, the default colors for IPython are *horrible*. Fortunately we can customize our IPython profile to fix them.

# Manage profiles

Check if you have an IPython profile and create one if needed with these commands:
- `ipython profile locate`
- `ipython profile list`
- `ipython profile create`

# Use vi editing mode
```python
# Search for and edit this line
c.TerminalInteractiveShell.editing_mode = 'vi'
```

# Fix colors in profile configuration

Open up `$(ipython profile locate)/ipython_config.py` for editing and make these changes
```python
from IPython.core.ultratb import VerboseTB
import IPython

# search for these options and edit them
c.TerminalInteractiveShell.highlighting_style = 'material'
c.TerminalInteractiveShell.true_color = True

# add this to the bottom of the file
# it replaces the awful ANSI yellow background with
# bold underlined text instead
VerboseTB._tb_highlight = 'bold underline'
```

# Remove long delay to exit insert mode
```python
# Add this line somewhere in ipython_config.py
c.TerminalInteractiveShell.emacs_bindings_in_vi_insert_mode = False
```

# Debugging

In an IPython session, you can run this magic to check configuration values
```python
%config TerminalInteractiveShell
```

You can also change config values on the fly for quick experimentation, for instance
```python
%config TerminalInteractiveShell.highlighting_style = 'monokai'
```

#python #shell