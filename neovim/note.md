# Neovim Note

## Plugins

### folke/noice.nvim

Filter notification by adding following:

```lua
opts = {
    -- ... other options
    filter = {
        event = "notify",
        find = "No active Snippet"
    }
}
```

To experience the `discomfort` mentioned above, try searching for assistance using `Telescope` but avoid entering the file.

Some events of `noice.nvim`:

- notify
- msg_show
- lsp
