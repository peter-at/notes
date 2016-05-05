## shallow clone
`git clone --depth 1` 

## .gitconfig/alias
```
[alias]
  st = status -uno
  stat = status
  ci = commit
  co = checkout
  d = diff
  l = log --pretty=format:\"%h %C(yellow)%ad%Creset %s %C(green)%an\" --decorate --abbrev-commit --date=short
  lv = log --oneline --pretty
  glog = log --graph
  logall = log --graph --branches=*
  ls = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate
  ll = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --numstat
  la = log --graph --branches=* --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]"
  grep = grep -Ii
  filelog = log -u
  diff = diff --word-diff
  dc = diff --cached
  r = reset
```

## .gitconfig/color
```
[color "branch"]
  current = yellow black
  local = yellow
  remote = magenta
[color "diff"]
  meta = yellow bold
  frag = magenta bold
  old = red reverse
  new = green reverse
  whitespace = white reverse
[color "status"]
  added = yellow
  changed = green
  untracked = cyan reverse
  branch = magenta
```
