# codex-starting-env

## Fix `lazy.nvim` / `luarocks` health errors on Windows Neovim

If `:checkhealth` shows errors like:

- `.../lazy-rocks/hererocks/bin/luarocks not installed`
- `.../lazy-rocks/hererocks/bin/lua version 5.1 not installed`

then `lazy.nvim` is trying to use the **hererocks** LuaRocks environment and failing.

### Recommended fix (Windows): disable LuaRocks support in `lazy.nvim`

In your `lazy.nvim` setup (usually `lua/config/lazy.lua`), set:

```lua
require("lazy").setup({
  -- your plugin specs
}, {
  rocks = {
    enabled = false,
    hererocks = false,
  },
})
```

This removes the health error and is safe unless you use plugins that explicitly depend on LuaRocks packages.

### Alternative (if you need LuaRocks)

Install a working Lua + LuaRocks toolchain on Windows and make sure `lua` and `luarocks` are on your `PATH`, then set:

```lua
rocks = {
  enabled = true,
  hererocks = false,
}
```

That tells `lazy.nvim` to use your system LuaRocks instead of the bundled hererocks environment.

### Apply and verify

1. Save your config.
2. Restart Neovim.
3. Run:
   - `:Lazy sync`
   - `:checkhealth lazy`

You should no longer see the hererocks/lua 5.1 error.
