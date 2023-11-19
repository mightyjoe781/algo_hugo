+++

Title = "tmux"

weight = 7

tags = ["screen","scripts","tmux"]

+++

tmux is another alternative to screen

Following contains some useful config and `.tmux.conf` for GNU screen.

##### Config

Note it uses same `Ctrl+a` prefix.

````c++
set -ga terminal-overrides ",xterm-256color*:Tc"

unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix
set -g status-style 'bg=#333333 fg=#5eacd3'

bind r source-file ~/.tmux.conf
set -g base-index 1

set-window-option -g mode-keys vi
bind -T copy-mode-vi v send-keys -X begin-selection
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'

# vim-like pane switching
bind -r ^ last-window
bind -r k select-pane -U
bind -r j select-pane -D
bind -r h select-pane -L
bind -r l select-pane -R

bind -r D neww -c "#{pane_current_path}" "[[ -e TODO.md ]] && nvim TODO.md || nvim ~/dotfiles/todo.md"
````

##### tmux Basics

Execute this command

```bash
tmux
```

This will create a screen session with name pts-0.hostname.

To detach the screen press `Ctrl+a d`

Most of commands follow `Ctrl+a` as a prefix.

Now since we are detached from screen we can list all currently running screens using `tmux ls`

We can name a new screen created using this command.

```bash
tmux new -s thecowmoos
```

This creates a screen session with name cowscream.

To reattach to a screen we can type 

```bash
tmux a -t <terminal_id>
```

- `Ctrl+a` `c` Create a new window (with shell)
- `Ctrl+a` `w` Choose window from a list
- `Ctrl+a` `0` Switch to window 0 (by number )
- `Ctrl+a` `,` Rename the current window
- `Ctrl+a` `%` Split current pane horizontally into two panes
- `Ctrl+a` `"` Split current pane vertically into two panes
- `Ctrl+a` `o` Go to the next pane
- `Ctrl+a` `;` Toggle between the current and previous pane
- `Ctrl+a` `x` Close the current pane
- `Ctrl+a` `?` Help on commands

