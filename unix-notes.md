# unix notes


# text processing

* print between 2 patterns - [link](./unix/awk-sed.md) 


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