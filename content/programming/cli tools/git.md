# Favorite .gitconfig

This config sets:
- name
- gitignore_global
- pretty colors
- **most importantly,** aliases

```
[user]
  email = your@email.com
  name = Your Name
[core]
  editor = vim
  excludesfile = ~/.gitignore_global
[color]
  ui = auto
[color "branch"]
  current = yellow bold
  local = green bold
  remote = cyan bold
[color "diff"]
  meta = yellow bold
  frag = magenta bold
  old = red bold
  new = green bold
  whitespace = red reverse
[color "status"]
  added = green bold
  changed = yellow bold
  untracked = red bold
[pager]
  branch = false
[alias]
  co = checkout
  cob = checkout -b
  del = branch -D
  cm = !git add -A && git commit -n -m
  wip = !git add -u && git commit -m "WIP"
  amend = commit -a --amend
  uncommit = reset --soft HEAD^
  unstage = reset HEAD --
  last = log -1 HEAD
  ls = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate
  ll = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --numstat
  le = log --oneline --decorate
  lg = log --graph --pretty=format:'%C(bold green)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
  logtree = log --graph --oneline --decorate --all
  fetch-tags = "!f() { : git fetch ; git fetch $1 'refs/tags/*:refs/tags/*'; }; f"
  defbranch = "!f() { git remote show origin | sed -n '/HEAD branch/s/.*: //p'; }; f"
  done = "!f() { : git checkout ; db=`git defbranch`; git co $db && git branch -d -r origin/$1 && git del $1 && git fetch origin $db && git merge origin/$db; }; f"
```

### Alias explanations
- `git defbranch` returns the default branch for this repo on origin, usually `main` or `master`
- `git done branch-name`  is used when a feature branch is merged into `main`. It deletes the feature branch and `git pull`s the `main` branch to update it.

# Tab completion

I've found that the tab completion that comes with `zsh` by default is sometimes slow and overly verbose. It also sometimes breaks the tab completion used by function-based git aliases, e.g.

```
done = "!f() { : git checkout ; db=`git defbranch`; git co $db && git branch -d -r origin/$1 && git del $1 && git fetch origin $db && git merge origin/$db; }; f"
```

Since I'm an oh-my-zsh user, I fixed this by using the `gitfast` plugin, which replaces `zsh`'s git completions and, under the hood, uses `bash`'s completions instead.

In my `~/.zshrc`:

```shell
plugins=(... gitfast)
```