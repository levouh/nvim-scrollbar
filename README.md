<div align="center">
  <h1>nvim-scrollbar</h1>
  <h5>Extensible Neovim Scrollbar</h5>
  <h1>🚧 WORK IN PROGRESS 🚧</h1>
  <p>This is a work in progress and breaking changes to the setup/config could
  occur in the future. Sorry for any inconveniences.
  </p>
</div>

![diagnostics](./assets/diagnostics.gif)

## Features

- Diagnostics
- Search (requires [nvim-hlslens](https://github.com/kevinhwang91/nvim-hlslens))

## Requirements

- Neovim >= 0.5.1
- [nvim-hlslens](https://github.com/kevinhwang91/nvim-hlslens) (optional)

## Installation

[vim-plug](https://github.com/junegunn/vim-plug)

```vim
Plug 'petertriho/nvim-scrollbar'
```

[packer.nvim](https://github.com/wbthomason/packer.nvim)

```lua
use("petertriho/nvim-scrollbar")
```

## Setup

```lua
require("scrollbar").setup()

```

### Search

![search](./assets/search.gif)

```lua
require("hlslens").setup({
    build_position_cb = function(plist, bufnr, changedtick, pattern)
        require('scrollbar').search_handler.show(plist.start_pos)
    end
})

vim.cmd([[
    augroup scrollbar_search_hide
      autocmd!
      autocmd CmdlineLeave : lua require('scrollbar').search_handler.hide()
    augroup END
]])
```

## Config

### Defaults

```lua
require("scrollbar").setup({
    handle = {
        text = " ",
        color = "white",
    },
    marks = {
        Search = { text = { "-", "=" }, priority = 0, color = "orange" },
        Error = { text = { "-", "=" }, priority = 1, color = "red" },
        Warn = { text = { "-", "=" }, priority = 2, color = "yellow" },
        Info = { text = { "-", "=" }, priority = 3, color = "blue" },
        Hint = { text = { "-", "=" }, priority = 4, color = "green" },
        Misc = { text = { "-", "=" }, priority = 5, color = "purple" },
    },
    render_filter = function(_, _)
        -- Passed `winid` and `bufnr` for the buffer in question, return `true`
        -- to draw the scrollbar, false otherwise
        --
        -- `exclude_filetypes` and `exclude_buftypes` are checked before this,
        -- set them to empty tables to skip those checks and rely solely on
        -- this function
        return true
    end,
    excluded_filetypes = {
        "",
        "TelescopePrompt",
        "startify",
        "NvimTree",
    },
    excluded_buftypes = {
        "prompt",
        "terminal",
    },
    autocmd = {
        render = {
            "BufWinEnter",
            "TabEnter",
            "TermEnter",
            "WinEnter",
            "CmdwinLeave",
            "TextChanged",
            "VimResized",
            "WinScrolled",
        },
    },
    handlers = {
        diagnostic = true,
        search = true,
    },
})
```

### Example config with [tokyonight.nvim](https://github.com/folke/tokyonight.nvim) colors

```lua
local colors = require("tokyonight.colors").setup()

require("scrollbar").setup({
    handle = {
        color = colors.bg_highlight,
    },
    marks = {
        Search = { color = colors.orange },
        Error = { color = colors.error },
        Warn = { color = colors.warning },
        Info = { color = colors.info },
        Hint = { color = colors.hint },
        Misc = { color = colors.purple },
    }
})
```

## Acknowledgements

- [kevinhwang91/nvim-hlslens](https://github.com/kevinhwang91/nvim-hlslens) for implementation on how to hide search results

## License

[MIT](https://choosealicense.com/licenses/mit/)
