# unix-notes.md

# distro - [link](./unix/distro.md)
* deb based (Ubuntu/Debian) [deb-based](./unix/distro.md#deb-based)


# shell
* get directory of script
```
#!/usr/bin/env bash
BASEDIR=$(dirname "$0")
echo "$BASEDIR"
```

## exit with exit code and message
```
err_exit() {
  local msg=$1
  echo "${msg}"
  exit ${2-1}
}
```

* trim whitespaces `echo "   lol  " | xargs` can be `xargs echo` to be explicit

* trim whitespace `echo -e " blah  " | sed -e 's/^[[:space:]]*//' -e 's/[[:space:]]*$//'`

* trim whitespaces using variable expansions
```
trim() {
    local var="$*"
    # remove leading whitespace characters
    var="${var#"${var%%[![:space:]]*}"}"
    # remove trailing whitespace characters
    var="${var%"${var##*[![:space:]]}"}"   
    printf '%s' "$var"
}
```

## version regex
```
$ regex='version:\s+(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)'; \
  echo "version: 0.1.2" \
  | grep -E "$regex" \
  | sed -E "s/${regex}/\1.\2.\3/"
0.1.2
$ regex='version:\s+(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)([-\+].*)?'; \
  echo "version: 0.1.2-beta+123" \
  | grep -E "$regex" \
  | sed -E "s/${regex}/\1.\2.\3 (pre-release or build-meta)\4/"
0.1.2 (pre-release or build-meta)-beta+123
```

## array
[doc ref](https://opensource.com/article/18/5/you-dont-know-bash-intro-bash-arrays)
[bunch of examples](https://tldp.org/LDP/abs/html/arrays.html)
```
declare -a arr
readarray -t arr < <(cmd-that-produce-list-on-each-line \
    | grep ^filtering \
    | cut -f 1)
```

# text processing

* print between 2 patterns - [link](./unix/awk-sed.md) 

* split file of words into word-per-line
  `tr -s '[[:punct:][:space:]]' '\n' < ll`
  `fmt -1 < ll`

## use printf
[source 1](https://linuxize.com/post/bash-printf-command/)
* type conversions `%[flags][width][.precision]specifier` [link](https://linuxize.com/post/bash-printf-command/#type-conversion-specifier)
```
# print 5 with at least 10 chars padded with 0
% printf "%0*d\n" 10 5
0000000005
# use -v to assign to var
% printf -v foo "hello"; echo $foo
hello
% printf "%.*f\n" 3 1.61803398
1.618
```


# tmux

* tmux - [link](./unix/tmux.md)


# terminal

* teminal colors - [link](./unix/terminal-console.md#colors)

* LSCOLORS/LS_COLORS - https://geoff.greer.fm/2008/06/27/lscolorsls_colors-now-with-linux-support/


# sys-admin

* adduser/addgroup vs useradd/groupadd - https://unix.stackexchange.com/a/55568
  * adduser/addgroup - interactive, useradd/groupadd - scripting


# num loops

* bash numbers
```
# odd numbers 1, 3, 5, 7, 9
% for n in {1..10..2}; do echo $n; done
# floating numbers inc by 0.8 from 3, 3.8, 4.6, 5.4
$ seq -f "%f" 3 0.8 6
# print all first of month for year 2021
% seq -wf "%02g/01/2021" 12
# sequence with 0 prefix 01 02 03... 
% seq -w 01 12 
# count downs
% seq 10 -1 1
```

# zsh

## see function def
```
zsh$ whence -f foo
foo () {
    echo hello
}

declare -f foo  # works in zsh and bash

typeset -f foo  # works in zsh, bash, and ksh

type -af  # zsh only (works differently in bash and ksh)
```

## not leaking internal functions/vars
```
function _mkprompt() {                                                      
  local var='%(?.%F{green}√.%F{red}?%?)%f %B%F{cyan}%1~%f%b %# '
  echo $var
}
PROMPT=`_mkprompt`
unfunction _mkprompt
```
or use anonymous function
```
function {            
  local show_rcode="%(?.%F{green}√.%F{red}?%?)%f"
  local show_path="%B%F{cyan}%1~%f%b"
  local show_hist="%F{green}%!"
  PROMPT="${show_rcode} ${show_path} ${show_hist}%# " 
}
```
