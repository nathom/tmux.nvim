# tmux.nvim

A *very* tiny plugin that lets you seamlessly navigate between tmux panes and vim splits.


# Installation

```lua
-- Using packer.nvim

use("nathom/tmux.nvim")

```

If you're a stickler for lazy loading like I am, use a dedicated
tmux mapping file in `~/.config/nvim/lua/config/tmux.lua` and
put the following in your plugin config instead:

```lua
use({ "nathom/tmux.nvim", config = [[require("config.tmux")]] })
```

# Usage

First and foremost, you need to add the following to your `.tmux.conf`

```tmux
# Smart pane switching with awareness of Vim splits.
# From https://github.com/christoomey/vim-tmux-navigator
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
| grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n Left if-shell "$is_vim" "send-keys Left" "select-pane -L"
bind-key -n Down if-shell "$is_vim" "send-keys Down" "select-pane -D"
bind-key -n Up if-shell "$is_vim" "send-keys Up" "select-pane -U"
bind-key -n Right if-shell "$is_vim" "send-keys Right" "select-pane -R"
```

`tmux.nvim` exposes 4 functions that you can map to any key of
your choosing—`move_left`, `move_right`, `move_up`, and `move_down`.

Then, put the following in your config:


```lua
local map = vim.api.nvim_set_keymap
map("n", "<Left>", [[<cmd>lua require('tmux').move_left()<cr>]])
map("n", "<Down>", [[<cmd>lua require('tmux').move_down()<cr>]])
map("n", "<Up>", [[<cmd>lua require('tmux').move_up()<cr>]])
map("n", "<Right>", [[<cmd>lua require('tmux').move_right()<cr>]])
```



If you don't want to use arrow keys, use the following template, replacing the `{side}` with the appropriate key name in Vim and tmux.

```tmux
# tmux.conf
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
| grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n {left} if-shell "$is_vim" "send-keys {left}" "select-pane -L"
bind-key -n {down} if-shell "$is_vim" "send-keys {down}" "select-pane -D"
bind-key -n {up} if-shell "$is_vim" "send-keys {up}" "select-pane -U"
bind-key -n {right} if-shell "$is_vim" "send-keys {right}" "select-pane -R"
```



```lua
-- init.lua or config/tmux.lua
local map = vim.api.nvim_set_keymap
map("n", "{left}", [[<cmd>lua require('tmux').move_left()<cr>]])
map("n", "{down}", [[<cmd>lua require('tmux').move_down()<cr>]])
map("n", "{up}", [[<cmd>lua require('tmux').move_up()<cr>]])
map("n", "{right}", [[<cmd>lua require('tmux').move_right()<cr>]])
```
