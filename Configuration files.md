## Fish Configuration file
```shell
if status is-interactive

# Commands to run in interactive sessions can go here

fish_add_path /opt/homebrew/bin

set --export FZF_DEFAULT_OPTS --exact

set fzf_directory_opts --bind "ctrl-o:execute(code {} &> /dev/tty)"

set fzf_history_opts "--nth=4.."

set fzf_fd_opts --hidden --exclude=.git

fzf_configure_bindings --git_log=\cl --git_status=\cs --directory=\ct --processes=\cp

alias reload-fish="source ~/.config/fish/config.fish"

end

# >>> conda initialize >>>

# !! Contents within this block are managed by 'conda init' !!

if test -f /Users/omack/miniconda3/bin/conda

eval /Users/omack/miniconda3/bin/conda "shell.fish" "hook" $argv | source

end

# <<< conda initialize <<<

source /Users/omack/.docker/init-fish.sh || true # Added by Docker Desktop
```
### Greeting file 
```shell
# ~/.config/fish/functions/fish_greeting.fish
function fish_greeting
	neofetch
end
```

