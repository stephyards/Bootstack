
## Introduction to Tmux

Tmux (Terminal Multiplexer) is an indispensable tool for power users, system administrators, and developers who require persistent, flexible, and efficient terminal session management. By enabling users to create, manage, and automate multiple terminal sessions within a single environment, Tmux significantly enhances productivity in command-line workflows.

### Key Advantages of Tmux

- **Session Persistence:** Retain active sessions even after SSH disconnections.
    
- **Multi-Window and Multi-Pane Management:** Organize terminal space efficiently.
    
- **Automation and Scripting:** Automate workflows with session initialization scripts.
    
- **Collaboration:** Share sessions with multiple users securely.
    
- **Customizability:** Tailor key bindings, colors, and behavior to personal preference.
    

## Installation and Setup

### Installing Tmux

#### Debian/Ubuntu:

```bash
sudo apt update && sudo apt install tmux -y
```

#### CentOS/RHEL:

```bash
sudo yum install tmux -y
```

#### macOS:

```bash
brew install tmux
```

### Verifying Installation

```bash
tmux -V  # Check installed version
```

## Core Tmux Workflow

### Creating and Managing Sessions




| Description                    | Command                                                       |
| ------------------------------ | ------------------------------------------------------------- |
| Start a new session            | `tmux`                                                        |
| Create a named session         | `tmux new -s <session_name>`                                  |
| List all active sessions       | `tmux ls` or `tmux list-sessions`                             |
| Reattach to a named session    | `tmux attach -t <session_name>` or `tmux a -t <session_name>` |
| Detach from a session          | `tmux detach` or `prefix d`                                   |
| Terminate a specific session   | `tmux kill-session -t <session_name>`                         |
| Terminate all running sessions | `tmux kill-server`                                            |
| Prefix                         | `CTRL-b`                                                      |
| Rename a session               | `tmux rename-session <session_name` or `prefix $`             |
| Switching sessions             | `prefix s`                                                    |

## Advanced Window and Pane Management

### Window Commands

| Description                   | Command                                          |
| ----------------------------- | ------------------------------------------------ |
| Create a new window           | `tmux new-window <window_name>` or `prefix c`    |
| Switch to the next window     | `prefix n`                                       |
| Switch to the previous window | `prefix p`                                       |
| Rename the current window     | `tmux rename-window <window_name>` or `prefix ,` |
| List all windows and navigate | `prefix w`                                       |
| Close the current window      | `prefix &`                                       |

### Pane Commands

| Command                              | Description                 |
| ------------------------------------ | --------------------------- |
| `prefix %` or `tmux split-window -h` | Split the pane vertically   |
| `prefix "` or `tmux split-window -v` | Split the pane horizontally |
| `prefix o`                           | Switch to the next pane     |
| `prefix q`                           | Display pane numbers        |
| `prefix x`                           | Close the current pane      |
| `prefix {`                           | Move pane left              |
| `prefix }`                           | Move pane right             |
| `prefix Space`                       | Toggle pane layouts         |

### Running commands on tmux   
Synchronizing Input Across Panes
```bash
prefix : # to switch to command mode
prefix : setw synchronize-panes on  # Enable
prefix : setw synchronize-panes off  # Disable
```

## Efficient Copy-Paste Workflow

| Command    | Description          |
| ---------- | -------------------- |
| `prefix [` | Enter copy mode      |
| `Space`    | Start selection      |
| `Enter`    | Copy selection       |
| `prefix ]` | Paste copied content |

## Tmux Customization and Automation

### Editing the Configuration File

```bash
vim ~/.tmux.conf

```

### Sample Configuration for Advanced Users

```bash
 # Enable mouse support
set -g mouse on
# use C-j and C-f for the prefix. 
set-option -g prefix C-j 
set-option -g prefix2 C-f 
unbind-key C-j bind-key C-j send-prefix 
set -g base-index 1 
# Use Alt-arrow keys without prefix key to switch panes 
bind -n M-Left select-pane -L 
bind -n M-Right select-pane -R 
bind -n M-Up select-pane -U 
bind -n M-Down select-pane -D 

# Set easier window split keys 
bind-key v split-window -h 
bind-key h split-window -v 

# Shift arrow to switch windows 
bind -n S-Left previous-window 
bind -n S-Right next-window 

# Easily reorder windows with CTRL+SHIFT+Arrow 
bind-key -n C-S-Left swap-window -t -1 
bind-key -n C-S-Right swap-window -t +1 

# Synchronize panes 
bind-key y set-window-option synchronize-panes\; display-message "synchronize mode toggled." 

# Easy config reload 
bind-key r source-file ~/.tmux.conf \; display-message "tmux.conf reloaded." 
# Set leader key to Ctrl-a
unbind C-b
set -g prefix C-a
bind C-a send-prefix


# Set automatic pane resizing
setw -g aggressive-resize on

# Customize the status bar
set -g status-bg colour235
set -g status-fg colour136
set -g status-left '[#S]'   # Display session name

#Initial setup
set -g default-terminal xterm-256color set -g status-keys vi 
# Easy clear history 
bind-key L clear-history 

# Key bindings for copy-paste 
setw -g mode-keys vi unbind p bind p paste-buffer bind-key -T copy-mode-vi 'v' 
send -X begin-selection bind-key -T copy-mode-vi 'y' send -X copy-selection-and-cancel 
# Mouse Mode 
set -g mouse on 
# Lengthen the amount of time status messages are displayed 
set-option -g display-time 3000 set-option -g display-panes-time 3000 
# Set the base-index to 1 rather than 0 
set -g base-index 1 set-window-option -g pane-base-index 1 
# Automatically set window title 
set-window-option -g automatic-rename on 
set-option -g set-titles on 
# Allow the arrow key to be used immediately after changing windows. 
set-option -g repeat-time 0 
# No delay for escape key press 
set -sg escape-time 0 
# Theme 
set-window-option -g window-status-current-style bold,bg=blue,fg=colour234 set-window-option -g window-status-style fg=colour35 set -g window-status-activity-style bold,bg=colour234,fg=white set-option -g message-style bg=colour237,fg=colour231 set-option -g pane-border-style fg=colour36 set-option -g pane-active-border-style fg=colour35 
# Change background color of a tab when activity occurs 
setw -g monitor-activity on 
# Do NOT reset the color of the tab after activity stops occuring 
setw -g monitor-silence 0 
# Disable bell 
setw -g monitor-bell off 
# Disable visual text box when activity occurs 
set -g visual-activity off 
# Status Bar set -g status-justify centre 
set -g status-bg black set -g status-fg colour35 set -g status-interval 60 set -g status-left-length 50 set -g status-left "#[bg=colour35]💻#[fg=colour234,bold] #H#[bg=colour34]#[bg=colour35,nobold]#[fg=colour234] [#S] $tmux_target_lower" set -g status-right '#[bg=colour35] 🕔 #[fg=colour234,bold]%H:%M '
```

### Apply Configuration Changes

```bash
tmux source-file ~/.tmux.conf
```

## Advanced Tmux Automation

### Automating Session Initialization

Create a startup script to launch preconfigured sessions:

```bash
echo 'tmux new-session -d -s work \
  && tmux new-window -t work:1 -n editor \
  && tmux split-window -h \
  && tmux select-pane -t 0 \
  && tmux attach -t work' > ~/start_tmux.sh
chmod +x ~/start_tmux.sh
```

Execute it with:

```bash
./start_tmux.sh
```

## Optimized Workflows and Best Practices

### Persistent Sessions with SSH

```bash
# Start a new persistent session
ssh user@server
export TMUX_SESSION="work"
tmux new -s $TMUX_SESSION

# Detach (Ctrl-b d) and reattach later
ssh user@server
tmux attach -t $TMUX_SESSION
```

### Efficient Scrolling and History Navigation

```bash
Ctrl-b [  # Enter scroll mode
Up/Down   # Scroll through history
q         # Exit scroll mode
```


## Conclusion

Tmux is an essential tool for advanced users who seek persistent and efficient terminal session management. By mastering session handling, automation, and customization, users can significantly enhance their productivity. Implementing the best practices outlined in this guide will streamline workflows and optimize terminal usage for any professional environment. Happy multiplexing!
