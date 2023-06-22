[Official site](https://github.com/tmux/tmux/wiki)

Configuration File  

The file must be in folder HOME (although can be changed)

`~/.tmux.conf`

1. Install [tpm](https://github.com/tmux-plugins/tpm)
```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
2. Copy configuration example (see below)
3. Source TPM 
```bash
# type this in terminal if tmux is already running
tmux source ~/.tmux.conf
```
4. Press `prefix` + I (capital i, as in **I**nstall) to fetch the plugin. 
	1. Prefix according to configuration below should be `ctrl + a`
### Configuration example

```configuration
### LOOK & FEEL ###
set-option -sa terminal-overrides ",xterm*:Tc"

#count windows and panes from 1
set -g base-index 1
setw -g pane-base-index 1

#using C-a as prefix
unbind C-b
set-option -g prefix C-a
bind C-a send-prefix

bind C-s choose-session

unbind |
unbind -
bind | split-window -hc "#{pane_current_path}"
bind - split-window -vc "#{pane_current_path}"

# Reload configuration 
bind-key r source-file ~/.tmux.conf \; display-message "Tmux config reloaded!"

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'sainnhe/tmux-fzf'

set -g @plugin 'noscript/tmux-mighty-scroll'
set -g mouse on

set -g @plugin 'tmux-plugins/tmux-resurrect'

# Theme settings
set -g @plugin 'catppuccin/tmux'
set -g @catppuccin_window_tabs_enabled on # or off to disable window_tabs
set -g @catppuccin_flavour 'latte' # or frappe, macchiato, mocha
set -g @catppuccin_date_time "%Y-%m-%d %H:%M"
set -g @catppuccin_host "on"

# Other examples:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'github_username/plugin_name#branch'
# set -g @plugin 'git@github.com:user/plugin'
# set -g @plugin 'git@bitbucket.com:user/plugin'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```

