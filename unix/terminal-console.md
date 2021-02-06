# terminal console (notes on terminal/console/unix command line)

# color
* msgcat --color=test |less -R
* bash scripts on [github](https://github.com/pvinis/colortools)
* bash simple loop
  ```
  for x in {0..8}; do
      for i in {30..37}; do
          for a in {40..47}; do
              echo -ne "\e[$x;$i;$a""m\\\e[$x;$i;$a""m\e[0;37;40m "
          done
          echo
      done
  done
  ```
* color doc from [bash-prompt](https://tldp.org/HOWTO/Bash-Prompt-HOWTO/x329.html)
  ```
  console fg
  Black       0;30     Dark Gray     1;30
  Blue        0;34     Light Blue    1;34
  Green       0;32     Light Green   1;32
  Cyan        0;36     Light Cyan    1;36
  Red         0;31     Light Red     1;31
  Purple      0;35     Light Purple  1;35
  Brown       0;33     Yellow        1;33
  Light Gray  0;37     White         1;37
  ```
* BG and FG
  ```
  T='gYw'   # The test text
  
  echo -e "\n                 40m     41m     42m     43m\
       44m     45m     46m     47m";
  
  for FGs in '    m' '   1m' '  30m' '1;30m' '  31m' '1;31m' '  32m' \
             '1;32m' '  33m' '1;33m' '  34m' '1;34m' '  35m' '1;35m' \
             '  36m' '1;36m' '  37m' '1;37m';
    do FG=${FGs// /}
    echo -en " $FGs \033[$FG  $T  "
    for BG in 40m 41m 42m 43m 44m 45m 46m 47m;
      do echo -en "$EINS \033[$FG\033[$BG  $T  \033[0m";
    done
    echo;
  done
echo
```
