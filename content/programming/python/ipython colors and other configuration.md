In recent version of IPython, the default colors are *horrible*. Fortunately we can customize our IPython profile to fix them.

# Manage profiles

Check if you have an IPython profile and create one if needed with these commands:
- `ipython profile locate`
- `ipython profile list`
- `ipython profile create`

Your profile will contain an `ipython_config.py` file. Configure it as follows:

# Use vi editing mode
```python
# enable vi editing mode
c.TerminalInteractiveShell.editing_mode = 'vi'
# eliminate the long delay when exiting insert mode
c.TerminalInteractiveShell.emacs_bindings_in_vi_insert_mode = False
```

# Fix colors in profile configuration

```python
from copy import deepcopy

from IPython.core.ultratb import VerboseTB
from IPython.utils.PyColorize import linux_theme, theme_table

# in ipython 9+, highlighting_style is deprecated in favor of themes
# unfortunately, there are not a lot of available themes at present
# instead, we create a one-dark theme ourselves
onedark = deepcopy(linux_theme)
onedark.base 'one-dark'
theme_table['one-dark'] = onedark
c.InteractiveShell.colors = 'one-dark'

# enable 24bit colors
c.TerminalInteractiveShell.true_color = True

# add this to the bottom of the file
# it replaces the awful ANSI yellow background with
# bold underlined text instead
VerboseTB.tb_highlight = 'bold underline'
VerboseTB.tb_highlight_style = 'default'
```

# Debugging

In an IPython session, you can run this magic to check configuration values
```python
%config TerminalInteractiveShell
```

You can also change config values on the fly for quick experimentation, for instance
```python
%config TerminalInteractiveShell.colors = 'linux'
```

#python #shell