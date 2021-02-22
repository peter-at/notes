# tmux

# using

* name window
tmux setw -g automatic-rename on
tmux setw -g automatic-rename-format
tmux setw rename-window new_name


# status bar

## reference - all in one
* source [link](https://arcolinux.com/everything-you-need-to-know-about-tmux-status-bar/)
* settings `tmux show-options -g | grep status`
* colors
  black, red, green, yellow, blue, magenta, cyan, white.
  bright colors, such as brightred, brightgreen, brightyellow, brightblue, brightmagenta, brightcyan.
  colour0 through colour255 from the 256-color set [reference](https://jonasjacek.github.io/colors/)
  default
  hexadecimal RGB code like #000000, #FFFFFF, similar to HTML colors.
  `$ tmux set-option status-style fg=white,bg=black`

## multiple session on the same window group
* source [link](https://gist.github.com/chakrit/5004006)
* start a session in tmux.conf
* start new-session in terminal targeting same group
* new-session set to destroy if non attached `set-option destroy-unattached`

## nested tmux
* source [link](https://www.freecodecamp.org/news/tmux-in-practice-local-and-nested-remote-tmux-sessions-4f7ba5db8795/)
* example tmux.conf [link](https://github.com/samoshkin/tmux-config/blob/master/tmux/tmux.conf)

## on %if
woks: `%if "#{==:t,t}"`, `%if "1"`
```
t="1"
%if "$t"
```
don't work: `%if "#(echo '1')"`

## conditional base on file
```
if-shell 'test -f file/path' {
  set-hook -g session-created "run file/path"
  set-hook -g session-closed "run file/path"
}
```
