<h1 align="center">🔖 fzf-bookmarks.yazi</h1>
<p align="center">
  <b>A lightning-fast, keyboard-first bookmark manager for <a href="https://github.com/sxyazi/yazi">Yazi</a></b><br>
  <i>Save, search, and jump to your favorite paths in a blink</i>
</p>

---

> [!NOTE]
> **This is a fork of [whoosh.yazi](https://github.com/WhoSowSee/whoosh.yazi)** by WhoSowSee
>
> This fork:
> - ✅ Fixes deprecated API warnings for Yazi v25+ (nightly builds)
> - ✅ Specifically targets Yazi nightly versions
> - 🚧 Extends functionality with new features (in development):
>   - Edit bookmark by key/fzf
>   - Add bookmark from search (fzf directory picker)

## Features

[Yazi](https://github.com/sxyazi/yazi) plugin for bookmark management, supporting the following features:

- **Persistent bookmarks** - No bookmarks are lost after you close yazi
- **Temporary bookmarks** - Session-only bookmarks that don't persist between restarts
- **Quick navigation** - Jump, delete, and rename bookmarks by keymap
- **Fuzzy search** - Support fuzzy search through [fzf](https://github.com/junegunn/fzf)
- **Multiple bookmark deletion** - Select multiple bookmarks with TAB in fzf
- **Configuration bookmarks** - Pre-configure bookmarks using Lua language
- **Smart path truncation** - Configurable path shortening for better readability
- **Directory history** - Navigate back to previous directory with Backspace
- **Tab history navigation** - Browse and jump to recently visited directories with Tab key
- **Quick bookmark creation** - Create temporary bookmarks directly from navigation menu
- **Configurable menu shortcuts** - Override the default Tab/Backspace/Enter/Space bindings from `init.lua`

## Installation

> [!IMPORTANT]
> Requires Yazi v25+ (nightly builds)

### Manual installation

```sh
# Linux/macOS
git clone https://github.com/yourusername/fzf-bookmarks.yazi ~/.config/yazi/plugins/fzf-bookmarks.yazi

# Or place in dev directory for local development
git clone https://github.com/yourusername/fzf-bookmarks.yazi ~/.config/yazi/dev/fzf-bookmarks.yazi
```

## Usage

Add this to your `init.lua`:

```lua
require("fzf-bookmarks"):setup({
  bookmarks = {
    -- Your bookmarks here
    h = "~",
    d = "~/Downloads",
    -- etc.
  },
  jump_notify = true,
  keys = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ",
})
```

For detailed configuration and keymap setup, see the [original whoosh.yazi documentation](https://github.com/WhoSowSee/whoosh.yazi).

### New Keymap Actions

Add these to your `keymap.toml`:

```toml
# Add bookmark from fzf directory search
{ on = ["b", "F"], run = "plugin fzf-bookmarks add_by_search", desc = "Add Bookmark from Search (fzf)" }

# Add temporary bookmark from fzf directory search
{ on = ["b", "C"], run = "plugin fzf-bookmarks add_temp_by_search", desc = "Add Temp Bookmark from Search (fzf)" }
```

## API Changes (v25+)

This fork replaces deprecated APIs:
- `cx.active` → `rt.active`
- `cx.tabs` → `rt.tabs`

These changes ensure compatibility with Yazi nightly builds and eliminate deprecation warnings.

## New Features

- ✅ **Add bookmark from fzf directory search** - Search and bookmark any directory under $HOME using fzf
  - Uses `fd` for fast directory traversal
  - Excludes common patterns (.git, node_modules, .cache)
  - Integrates with existing tag/key bookmark creation flow
  - Supports both permanent and temporary bookmarks

## Planned Features

- [ ] Edit bookmark by key
- [ ] Edit bookmark by fzf

## Credits

Original plugin by [WhoSowSee](https://github.com/WhoSowSee) - [whoosh.yazi](https://github.com/WhoSowSee/whoosh.yazi)

## License

See [LICENSE](LICENSE)
