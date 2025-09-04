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

# Enable mouse support
set -g mouse on

# Load tmux-logging plugin
set -g @plugin 'tmux-plugins/tmux-logging'

# Status bar color
set -g status-bg colour27
set -g status-fg white

# Pane management bindings
bind-key j command-prompt -p "Join pane from:" "join-pane -s '%%'"
bind-key s command-prompt -p "Send pane to:" "join-pane -t '%%'"

# Use vi keybindings in copy mode
set -g mode-keys vi

# Automatically copy text to clipboard after selection
bind -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe "xclip -selection clipboard"

# Ensure the plugin runs correctly
run-shell /opt/tmux-logging/logging.tmux
```
- __sudo apt install xclip__

- __Create a new session__
  - __`tmux new -s HTB`__
- __Check tmux is working__
  - __`Ctrl+a then press i`__
- __background and close__
  - __`Ctrl+a then press x`__ to close
  - __`Ctrl+a then press d`__ to keep in background

- __To list the tmux sessions__
  - __`tmux ls`__

- __To open the tmux session__
  - __`tmux attach`__
  - __`tmux attach -t <seesion_name>`__

- __List all the sessions and tabs__
  - __`Ctrl + a w`__



- __`sudo git clone https://github.com/tmux-plugins/tmux-logging.git /opt/tmux-logging/`__
- __`tmux source-file ~/.tmux.conf`__

### 1. To record the logs
  - __Start__
    - __`ctrl+a shift+p`__
  - __Stop__
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

- To resize that split screen
  - Ctrl+a then press hold Ctrl < (left arrow <---)

- To split the screen vertical
  - Ctrl+a shitf+"


- To move between split screen
  - Ctrl+a the use arrow to move
  - Ctrl+a then press any arrow up,down,left,right
 
- Copy text
  - Enter copy mode
    - `Ctrl+a [`
    - Move the cursor with arrow keys to the start of the text.
    - Press Space to begin selection.
    - Use the arrow keys to highlight the text.
    - Press Enter to copy the selection.

- Past text
  - Ctrl+a ]

- Kill all sessions at once
  - `tmux kill-server`
    
- Kill a specific session by name
  - `tmux kill-session -t <session_name>`

- Kill a single pane
  - Ctrl+a x
 
- Kill the entire window (all panes inside it)
  - Ctrl+a Shift+&


