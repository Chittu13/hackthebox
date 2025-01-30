## tmux setup

- __sudo nano .tmux.conf__

```
# Set prefix to Ctrl+A
set -g prefix C-a
bind C-a send-prefix
unbind C-b

# Set history limit and prevent renaming
set -g history-limit 1000000
set -g allow-rename off

# Load tmux-logging plugin
set -g @plugin 'tmux-plugins/tmux-logging'

# Status bar color
set -g status-bg colour27
set -g status-fg white

# Pane management bindings
bind-key j command-prompt -p "Join pane from:" "join-pane -s '%%'"
bind-key s command-prompt -p "Send pane to:" "join-pane -t '%%'"

# Use vi keybindings in copy mode
set-window-option -g mode-keys vi

# Ensure the plugin runs correctly
run-shell /opt/tmux-logging/logging.tmux
```

- __Check tmux is working__
  - __`Ctrl+a then press i`__

- __`sudo git clone https://github.com/tmux-plugins/tmux-logging.git /opt/tmux-logging/`__


- __`tmux source-file ~/.tmux.conf`__

- __`ctrl+a shift+p`__ 


- To rename the tab 
  - Ctrl+a then press ,


- To Create a new tab 
  - Ctrl+a then press c

 
- To shitf between tab
  - Ctrl+a then press the number of the tab like 
  - Ctrl+a then press 0

- To split the screen horizontal 
  - Ctrl+a shitf+%

- To close split screen 
  - Ctrl+a then press x 
  - or 
  - exit

- To resize that split screen
  - Ctrl+a then press hold Ctrl < (left arrow <---)

- To split the screen vertical
  - Ctrl+a shitf+"


- To move between split screen
  - Ctrl+a the use arrow to move
  - Ctrl+a then press any arrow up,down,left,right

