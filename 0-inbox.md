# inbox


## document writing

- https://github.com/tindzk/thesaurus
- https://www.writethedocs.org/guide/
- https://docs.divio.com/documentation-system/



## tmux conf
```

# tmux.conf


# all important prefix key
# - default C-b is fine (in vim C-b == C-b C-b or C-u C-u)
set-option -g prefix C-b
set-option -g prefix2 None


# status-line
set-option -g status on
#set -g status-style bg=colour40
#setw -g window-status-current-style bg=colour40
# Set status bar
set -g status-style bg=black,fg=white
set -g status-left ""
set -g status-right "#[fg=green]#H"


# server
set-option -s buffer-limit 2
set-option -s escape-time 0
set-option -s history-file ${HOME}/.cache/tmux/cmd-history

# session
set-option -g base-index 1

# window
set-option -wg aggressive-resize on

## copy mode
set-option -wg mode-keys vi
bind-key -T copy-mode-vi 'v' send-keys -X begin-selection
bind-key -T copy-mode-vi 'y' send-keys -X copy-pipe-and-cancel

## visual

# visual
bind -n C-t new-window -a
bind -n M-F11 set -qg status-style bg=colour25
bind -n M-F12 set -qg status-style bg=colour40
#bind -n S-up \
#	send-keys M-F12 \; \
#	set -qg status-bg colour25 \; \
#	unbind -n S-left \; \
#	unbind -n S-right \; \
#	unbind -n S-C-left \; \
#	unbind -n S-C-right \; \
#	unbind -n C-t \; \
#	set -qg prefix C-b
#bind -n S-down \
#	send-keys M-F11 \; \
#	set -qg status-bg colour40 \; \
#	bind -n S-left  prev \; \
#	bind -n S-right next \; \
#	bind -n S-C-left swap-window -t -1 \; \
#	bind -n S-C-right swap-window -t +1 \; \
#	bind -n C-t new-window -a -c "#{pane_current_path}" \; \
#	set -qg prefix C-a

### large history
##set-option -g history-limit 5000
##
### set to be VI like
##set-window-option -g mode-keys vi
##set-option -g status-keys vi
###... cut and paste
###unbind-key [
###bind-key Escape copy-mode
###unbind-key p
###bind-key p paste-buffer
##bind-key = choose-buffer
##bind-key [ copy-mode
##bind-key ] paste-buffer
##bind-key b copy-mode -u
###... for splitting
##bind-key s split-window -v
##bind-key v split-window -h
###... for switching between pane
##bind-key h select-pane -L
##bind-key j select-pane -D
##bind-key k select-pane -U
##bind-key l select-pane -R
###... for resize pane
##bind-key < resize-pane -L 1
##bind-key > resize-pane -R 1
###... for switching between windows
##bind-key n next-window
##bind-key p previous-window
##bind-key C-_ last-window
##
### mouse... does not work 01/20/13 (missing mouseterm and simbl?)
###set-option -g mode-mouse on
###set-option -g mouse-select-pane on
###set-option -g mouse-select-window on
###set-window-option -g mode-mouse on
##
### session/window and apperances
###... set title to <session name>:<window idx> - <path>
##set-option -g set-titles on
##set-option -g set-titles-string '[#S:#I]'
###... coloring
##set-option -g default-terminal "screen-256color" 
####set-option -g pane-active-border-fg blue
###... numbering - set to base on keyboard layout
##set-option -g base-index 1
##set-option -g pane-base-index 1
##
### status bar
#### set-option -g status-utf8 on
##set-option -g status-bg black
##set-option -g status-fg white
##set-option -g status-justify centre
###... left/center/right
##set-option -g status-left '#[fg=green][#[bg=black,fg=cyan]#S#[fg=green]]'
##set-option -g status-left-length 20
##set-window-option -g automatic-rename on
##set-window-option -g window-status-format \
##    '#[fg=cyan,dim]#I#[fg=blue]:#[default]#W#[fg=grey,dim]#F'
##set-window-option -g window-status-current-format \
##    '#[bg=blue,fg=cyan,bold]#I#[bg=blue,fg=cyan]:#[fg=colour230]#W#[fg=dim]#F'
##set-option -g status-right \
##    '#[fg=green][#[fg=blue]%Y-%m-%d #[fg=white]%H:%M#[default] #[fg=green]]'


# default to start a new-session, such that `tmux attach-session` will
# will create a session if non exists
new-session -As def -c ${HOME}
```

```


# split panes with | and -
bind | split-window -h
bind - split-window -v

# select panes with vi keys
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# enable mouse
set -g mouse on
## double click and triple click copies
### Double LMB Select & Copy (Word)
bind-key -T copy-mode-vi DoubleClick1Pane \
    select-pane \; \
    send-keys -X select-word \; \
    send-keys -X copy-pipe "xclip -in -sel primary"
bind-key -n DoubleClick1Pane \
    select-pane \; \
    copy-mode -M \; \
    send-keys -X select-word \; \
    send-keys -X copy-pipe "xclip -in -sel primary"
### Triple LMB Select & Copy (Line)
bind-key -T copy-mode-vi TripleClick1Pane \
    select-pane \; \
    send-keys -X select-line \; \
    send-keys -X copy-pipe "xclip -in -sel primary"
bind-key -n TripleClick1Pane \
    select-pane \; \
    copy-mode -M \; \
    send-keys -X select-line \; \
    send-keys -X copy-pipe "xclip -in -sel primary"

# window/pane index
set -g base-index 1
setw -g pane-base-index 1

# enable vi keys
setw -g mode-keys vi
bind Escape copy-mode
bind-key -T copy-mode-vi v send -X begin-selection
bind-key -T copy-mode-vi y send -X copy-selection
unbind p
bind p paste-buffer
```

```

# switch prefix from 'C-b' to 'C-a'
set-option -g prefix C-a
set-option -g prefix2 C-_
unbind-key C-b
bind-key C-a send-prefix

# large history
set-option -g history-limit 5000

# set to be VI like
set-window-option -g mode-keys vi
set-option -g status-keys vi
#... cut and paste
#unbind-key [
#bind-key Escape copy-mode
#unbind-key p
#bind-key p paste-buffer
bind-key = choose-buffer
bind-key [ copy-mode
bind-key ] paste-buffer
bind-key b copy-mode -u
#... for splitting
bind-key s split-window -v
bind-key v split-window -h
#... for switching between pane
bind-key h select-pane -L
bind-key j select-pane -D
bind-key k select-pane -U
bind-key l select-pane -R
#... for resize pane
bind-key < resize-pane -L 1
bind-key > resize-pane -R 1
#... for switching between windows
bind-key n next-window
bind-key p previous-window
bind-key C-_ last-window

# mouse... does not work 01/20/13 (missing mouseterm and simbl?)
#set-option -g mode-mouse on
#set-option -g mouse-select-pane on
#set-option -g mouse-select-window on
#set-window-option -g mode-mouse on

# session/window and apperances
#... set title to <session name>:<window idx> - <path>
set-option -g set-titles on
set-option -g set-titles-string '[#S:#I]'
#... coloring
set-option -g default-terminal "screen-256color" 
set-option -g pane-active-border-fg blue
#... numbering - set to base on keyboard layout
set-option -g base-index 1
set-option -g pane-base-index 1

# status bar
set-option -g status-utf8 on
set-option -g status-bg black
set-option -g status-fg white
set-option -g status-justify centre
#... left/center/right
set-option -g status-left '#[fg=green][#[bg=black,fg=cyan]#S#[fg=green]]'
set-option -g status-left-length 20
set-window-option -g automatic-rename on
set-window-option -g window-status-format \
    '#[fg=cyan,dim]#I#[fg=blue]:#[default]#W#[fg=grey,dim]#F'
set-window-option -g window-status-current-format \
    '#[bg=blue,fg=cyan,bold]#I#[bg=blue,fg=cyan]:#[fg=colour230]#W#[fg=dim]#F'
set-option -g status-right \
    '#[fg=green][#[fg=blue]%Y-%m-%d #[fg=white]%H:%M#[default] #[fg=green]]'

#
# session...
#
source-file ~/.tmux/basic.session
```

```

# tmux.conf


# all important prefix key and toggle logic
# - default C-b is fine (in vim C-b == C-b C-b or C-u C-u)
set-option -g prefix C-b
set-option -g prefix2 None
bind -T root F11 \
  set prefix None \;\
  set key-table off \;\
  set status-style "fg=black,bg=green" \;\
  set window-status-current-format "#I:#W" \;\
  set window-status-current-style "fg=white,bold,bg=green" \;\
  if -F '#{pane_in_mode}' 'send-keys -X cancel' \;\
  refresh-client -S

bind -T off F11 \
  set -u prefix \;\
  set -u key-table \;\
  set -u status-style \;\
  set -u window-status-current-style \;\
  set -u window-status-current-format \;\
  refresh-client -S

# status line
set-option -g status on
set-option -g status-style bg=black,fg=white
set-option -g status-position top
set-option -g status-left "[#S] "
set-option -g status-left-length 10
set-option -g status-right "#[fg=green]#H"
set-option -g window-status-current-style "bg=colour236"
set-option -g status-interval 45

# window/pane index
set-option -g base-index 1
set-option -g pane-base-index 1


# split panes with | and -
bind-key | split-window -h
bind-key - split-window -v

# select panes with vi keys
bind-key h select-pane -L
bind-key j select-pane -D
bind-key k select-pane -U
bind-key l select-pane -R

# enable mouse
set -g mouse on
## double click and triple click copies
### Double LMB Select & Copy (Word)
bind-key -T copy-mode-vi DoubleClick1Pane \
    select-pane \; \
    send-keys -X select-word \; \
    send-keys -X copy-pipe "xclip -in -sel primary"
bind-key -n DoubleClick1Pane \
    select-pane \; \
    copy-mode -M \; \
    send-keys -X select-word \; \
    send-keys -X copy-pipe "xclip -in -sel primary"
### Triple LMB Select & Copy (Line)
bind-key -T copy-mode-vi TripleClick1Pane \
    select-pane \; \
    send-keys -X select-line \; \
    send-keys -X copy-pipe "xclip -in -sel primary"
bind-key -n TripleClick1Pane \
    select-pane \; \
    copy-mode -M \; \
    send-keys -X select-line \; \
    send-keys -X copy-pipe "xclip -in -sel primary"

# enable vi keys
setw -g mode-keys vi
bind Escape copy-mode
bind-key -T copy-mode-vi v send -X begin-selection
bind-key -T copy-mode-vi y send -X copy-selection
unbind p
bind p paste-buffer
```

# jenkins

## update variable in pipeline
```
def awesomeVersion = 'UNKNOWN'

pipeline {
  agent { label 'docker' }
  stages {
    stage('build') {
      steps {
        script {
          awesomeVersion = sh(returnStdout: true, script: 'echo 0.0.1')
        }
      }
    }
    stage('output_version') {
      steps {
        echo "awesomeVersion: ${awesomeVersion}"
      }
    }
  }
}
```

# mysql

## show permission
mysql> SHOW GRANTS FOR 'user'@'localhost';
select * from mysql.user where User='user' \G;

# makefile

* [check defined](https://stackoverflow.com/a/10858332)
```make
check_defined=$(if $(value $1),,$(error Err: undefined variable: $1))
tgt:
  @:$(call check_defined,undef_var)
  
list-to-sh: LIST:=one two three
list-to-sh:
    @arr=( $(LIST) ); for n in "$${arr[@]}"; do echo $$n; done
```

```
MAKEFLAGS += --warn-undefined-variables
SHELL := bash
.SHELLFLAGS := -eu -o pipefail -c
.DEFAULT_GOAL := all
.DELETE_ON_ERROR:
.SUFFIXES:
```


# python

## template jinjia2 vs mako

[syntax compare](https://stackoverflow.com/a/3436091)
+1 jinjia2 - syntax


# links

## searching github
https://sourcegraph.com/search
https://grep.app/

## check port in use
.. from https://www.cyberciti.biz/faq/unix-linux-check-if-port-is-in-use-command/
sudo lsof -i -P -n | grep LISTEN
sudo netstat -tulpn | grep LISTEN
sudo ss -tulpn | grep LISTEN
sudo lsof -i:22 ## see a specific port such as 22 ##
sudo nmap -sTU -O IP-address-Here

## check port stats (sending)
https://unix.stackexchange.com/questions/56988/how-can-i-find-which-process-sends-data-to-a-specific-port
```
sudo ss -tp
# or
sudo netstat -A inet -p | grep -v localhost
```

# pandas

## epoch to datetime
```python
df.assign(
    dt=lambda x: pd.to_datetime(df.epoch_time, unit='s')
    dt_tz=lambda x: pd.to_datetime(df.epoch_time, unit='s').dt.tz_localize('UTC').dt.tz_convert('America/New_York')
)
df['dt']=df['epoch_time'].apply(lambda x: dt.datetime.fromtimestamp(x))
```
