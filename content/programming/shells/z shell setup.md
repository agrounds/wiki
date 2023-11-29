# Install

Use whatever method your OS supports to install z shell. Then set it as your default login shell.

#### Examples
**NetBSD**
```sh
pkgin install zsh
which zsh
chsh  # change shell for your user
```

Log out and back in. Z shell will ask you to set some preferences. Set them and continue.

# Customize prompt

There are various ways to set your prompt, including third party tools such as Oh My Zsh (which does a lot more than just prompt customization). Below, we give an example of a simple colorful prompt which can be customized as needed.

**~/.zshrc**
```zsh

export PROMPT='%F{cyan}%n%f%F{white}@%f%U%F{magenta}%B%m%b%f%u %F{070}%B%%%b%f '
export RPROMPT='%B%F{214}%~%f%b'
```

#### Explanation
There are two variables used here: `PROMPT` and `RPROMPT`, used for setting the left and right sides of the prompt respectively. 

The `%F{color}...%f` instruction tells Z shell to set the color of the text. Similarly `%U...%u` and `%B...%b` tell it to make the text underlined or bold. Colors can be black, red, green, yellow, blue, magenta, cyan or white. Or, alternatively, you can use a number from the [[Xterm_256color_chart.svg|xterm-256color chart|]].

The `%%` instruction gives a literal `%`. The `%~` instruction gives the current working directory. Other instructions are available as well. See the [Z shell manual](https://man.archlinux.org/man/zshmisc.1#Visual_effects) for more information.

# Auto completion

This should be set when you first run z shell, but to be sure, this should be in your `~/.zshrc`:
```zsh
autoload -Uz compinit
compinit
```

To enable arrow key navigation in the tab completion menu, add this
```zsh
zstyle ':completion:*' menu select
```

# More .zshrc configuration
### Modifying PATH
```zsh
# append
path+=('/home/david/pear/bin')
# or prepend
path=('/home/david/pear/bin' $path)
# export to sub-processes (make it inherited by child processes)
export PATH
```

# Resources
- [Arch Linux wiki entry on z shell](https://wiki.archlinux.org/title/zsh)