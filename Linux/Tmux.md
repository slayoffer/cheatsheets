### Basics

```bash
sudo apt-get install tmux

# check version
tmux -V 

# new session
tmux
# or
tmux new

# new session with name
tmux new -s myproject

# close session
exit

# sessions list
tmux ls
# or
tmux list-sessions

# attach session
tmux a
# or
tmux attach -t 1 # session 1
# or
tmux a -t 0 # will attach last session
# or
tmux attach

# kill session
tmux kill-session -t <name>

# kill all but the current
tmux kill-session -a
```

### Prefix

```bash
Ctrl+b
```

### Split panes

```bash
# horizontal
Prefix + "

# vertical
Prefix + %

# move between panes
Prefix + arrow keys

# zoom/unzoom pane (toggle)
Prefix + z
```

### Windows

```bash
# new session prefix
Prefix + c

# kill pane
Prefix + x

# kill window
Prefix + &

# move to next
Prefix + n

# move to previous
Prefix + p

# rename window
Prefix + ,

# resize windows
Prefix + Ctrl + arrow keys
```

### Sessions

```bash
# detach
Prefix + d

# rename
Prefix + $

# GUI list sessions
Prefix + s
```

### Config example

```bash
vim ~/.tmux.conf

# extra prefix
set-option -g prefix C-a # Ctrl + a for prefix
unbind-key C-a
bind-key C-a send-prefix
 
# Use Alt-arrow keys to switch panes
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
 
# Shift arrow to switch windows
bind -n S-Left previous-window
bind -n S-Right next-window
 
# Mouse mode
setw -g mouse on
 
# Set easier window split keys
bind-key v split-window -h # Prefix + h for horizontal
bind-key h split-window -v
 
# Hot config reload
bind-key r source-file ~/.tmux.conf \; display-message "~/.tmux.conf reloaded."
```

