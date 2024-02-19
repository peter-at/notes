# git-notes

## revert last commit
```
$ git reset HEAD^
# revert and remove local changes
$ git reset --hard HEAD^
```

## undo add
`git reset -- file.c`

## clean local files
```
# show files and directories that git does not know about and plan to remove
$ git clean -x -d -n
```

## delete tag
```
$ git tag -d atag
atag deleted
$ git push origin :atag
```

## create and apply patch
```
# create
$ git format-patch HEAD^

# apply
$ git am ../patch-from-above
```

## switch to main
```
$ git fetch upstream main
$ git co upstream/main
$ git push -u origin/main
```

## show branches with commit hash and description
```
$ git for-each-ref --sort=committerdate refs/heads/ --format='%(HEAD) %(color:yellow)%(refname:short)%(color:reset):%(objectname:short) - %(contents:subject)'
$ git config --global alias.brls "for-each-ref --sort=committerdate refs/heads/ --format='%(HEAD) %(color:yellow)%(refname:short)%(color:reset):%(objectname:short) - %(contents:subject)'"
```

## gitignore
```
# show files git does not know about (.gitignore has effect)
$ git ls-files --other

# show files git are ignoring
$ git ls-files --ignored --exclude-standard
```

## access token in gitconfig
```
[credential "https://github.../"]
  username = token
  helper = "!f() { echo 'password=token'; }; f"
```

## multiple pathspec 
```
# ex: grep pat in entire repo ':/', not a paritcular directory ':!not_this_dir' and not headers ':!*.h'
$ git grep pat -- ':/' ':!not_this_dir' ':!*.h'
```

## shallow clone
`git clone --depth 1` 
.. ref https://github.blog/2020-12-21-get-up-to-speed-with-partial-clone-and-shallow-clone/
.. blob less clone
git clone --filter=blob:none <url>
.. tree less clone
git clone --filter=tree:0 <url>
.. shallow clone
git clone --depth=1 <url>
.. list filter matches... list of filters in man git-rev-list
.. https://git-scm.com/docs/git-rev-list#Documentation/git-rev-list.txt---filterltfilter-specgt
git rev-list --filter=blob --all

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

## clone a single branch and disable push

```
git clone --single-branch --branch a_branch https://github.com/user/repo.git
git remote set-url --push origin no_push
```


## pull requests manange
```
  515  git co -b refresh_oct
  530  git fetch upstream pull/226/head:pr226
  531  git fetch upstream pull/225/head:pr225
  532  git fetch upstream pull/227/head:pr227
  533  git branch -l
  534  git merge pr225
  535  git merge pr226
  682  git fetch upstream
  683  git cherry-pick upstream/pr/228

$ grep -A3 'remote "upstream' .git/config
[remote "upstream"]
	url = <a url>
	fetch = +refs/heads/*:refs/remotes/upstream/*
  fetch = +refs/pull/*/head:refs/remotes/upstream/pr/*
```
