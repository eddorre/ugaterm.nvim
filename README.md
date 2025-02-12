# ugaterm.nvim

A terminal plugin for Neovim.

See help for details.
### Introduction
A terminal plugin for Neovim.

Requirements: Neovim. It may work with older Neovim, only the latest stable release is guaranteed to work.
### Installation

#### Using Vimplug

Add the following to your `init.vim` file:

```vim
Plug 'uga-rosa/ugaterm.nvim'

" Ensure the plugin is loaded after installation
autocmd VimEnter * lua require("ugaterm").setup({})
```

Then reload Neovim and run `:PlugInstall`
#### Using Lazyvim

Create a `ugaterm.lua` file in your Neovim plugins directory. Add the following:

```lua
return {
  'lazyvim/ugaterm.nvim',
}
```
#### Using Packer

Add the following to your `init.lua` file:
```lua
use {
  "uga-rosa/ugaterm.nvim",
  config = function()
    require("ugaterm").setup({})
  end
}
```

Then reload Neovim and run `:PackerSync`
### Commands

`:UgatermOpen [{flag}] [{}-name}] [{cmd}]`

Opens the most recently used terminal. If no terminal exists, it creates a new one.

You can also specify a terminal with `{name}`. This is also used for newly created terminals. If omitted, the default name will be prefix+index.

`{cmd}` is the command sent to the terminal. If `[range]` is given, the buffer in that range (in rows) as `{cmd}` and any specified `{cmd}` is ignored.

Available {flag} options are:

`-new` - Creates a new terminal instead of using the most recently used one.
`-toggle` - If the terminal is already open, it will be closed.
`-select` - Uses `vim.ui.select()` to select the terminal to open.
`-keep_cursor` - Open a terminal without moving the cursor.

`:UgatermHide`

By default, it only hides the terminal like `:hide`. The -delete flag deletes the buffer (`:bwipeout`).

`:UgatermSend [-name {name}] {cmd}`

Send `{cmd}` to the `{name}` terminal. If `{name}` is omitted, it is sent to the most recently used terminal.

`:UgatermRename [-target {name}] {newname}]`

Rename the `{name}` terminal to `{newname}`.

If the `{name}` is omitted, the target is the most recently used terminal.

If `{newname}` is omitted, `vim.ui.input()` is used.
### Options

Use the `setup()` function to set options. If you do not change the default settings, you do not need to call `setup()`.

`prefix [string]`

The terminal buffer name prefix. Default is `terminal://`.

`filetype [string]`

The filetype for a terminal buffer. Default is `ugaterm`.

`open_cmd [string | function]`

The command / function to open a terminal window. Default is `botright 15sp`.

Example of opening in a floating window:
```lua
return {
  'uga-rosa/ugaterm.nvim',
  config = function()
    require("ugaterm").setup({
      open_cmd = function()
        local height = vim.api.nvim_get_option("lines")
        local width = vim.api.nvim_get_option("columns")
        vim.api.nvim_open_win(0, true, {
          relative = "editor",
          row = math.floor(height * 0.1),
          col = math.floor(width * 0.1),
          height = math.floor(height * 0.8),
          width = math.floor(width * 0.8),
        })
      end,
    })
  end,
}
```
### AUTOCMDS

`UgatermEnter`

After entering the ugaterm window. Fires only when moved by command.

`UgatermLeave`

Before leaving the ugaterm window. Fires only when moved by command.
