# fzf-bookmarks.yazi - New Features Documentation

## Overview
This enhanced version of fzf-bookmarks.yazi includes several powerful new features requested by the user.

## New Features

### 1. Separate Rename and Reassign Key Functions

Previously, renaming a bookmark would require re-entering both the tag (name) and the key (shortcut). Now these are separated:

#### Rename Tag Only (Keep Existing Key)
- **By Key**: `b` `k` `r` - Select bookmark by key menu, then rename just the tag
- **By FZF**: `b` `f` `r` - Select bookmark via fzf, then rename just the tag

#### Reassign Key Only (Keep Existing Tag)
- **By Key**: `b` `k` `k` - Select bookmark by key menu, then reassign just the shortcut
- **By FZF**: `b` `f` `k` - Select bookmark via fzf, then reassign just the shortcut

**Benefits**: Faster workflow when you only want to change the name OR the shortcut, not both.

---

### 2. Quick Group Switching with Numbered Shortcuts

In addition to the existing FZF-based group switcher, there's now a quick switcher that shows numbered options:

- **Quick Switch**: `b` `g` `g` - Shows a numbered menu (1-9) for instant group selection
- **FZF Switch**: `b` `g` `s` - Original FZF-based group selector (still available)

**Benefits**:
- Faster switching for users with 9 or fewer bookmark groups
- Visual shortcut table makes it easy to remember which number corresponds to which group
- Active group is marked with `*`

---

### 3. Global Scope Bookmarks (Auto-Generated Keymaps)

This is the most significant new feature. Previously, bookmark shortcuts only worked when you entered the bookmark plugin interface (`b` prefix). Now you can have **true global scope bookmarks** that work from anywhere in Yazi!

#### How It Works

The plugin automatically generates a `bookmarks-global.toml` file containing direct `cd` keybindings for all your bookmarks.

**File Location**: `$YAZI_CONFIG_HOME/.data/bookmarks-global.toml`

#### Setup Instructions

1. **Automatic Generation**: The global keymap file is auto-generated whenever you:
   - Add a bookmark
   - Rename a bookmark
   - Reassign a bookmark key
   - Delete a bookmark

2. **Include in your keymap.toml**: Add this line at the top of your `keymap.toml`:
   ```toml
   #:include .data/bookmarks-global.toml
   ```

3. **Manual Generation**: You can manually trigger generation with:
   - **Keybind**: `b` `G` `g`
   - This will show a notification with the path to the generated file

#### Example Generated Keybinding

If you have a bookmark:
- **Tag**: "WezTerm Config"
- **Key**: `["g", "w"]`
- **Path**: `/home/user/.config/wezterm`

The generated keybinding will be:
```toml
{ on = ["g", "w"], run = "cd \"/home/user/.config/wezterm\"", desc = "Go to: WezTerm Config" }
```

Now pressing `g` `w` anywhere in Yazi instantly jumps to that directory!

#### Configuration Option

You can disable auto-generation in `init.lua`:

```lua
fzf_bookmarks:setup({
  auto_generate_global_keymap = false,  -- Disable auto-generation
  -- ... other options
})
```

**Benefits**:
- **No Plugin Prefix Required**: Jump directly with your bookmark shortcuts (e.g., `g` `w` instead of `b` `f` `g` `w`)
- **Faster Navigation**: One less keypress for every bookmark jump
- **Familiar Workflow**: Works just like Yazi's built-in `g` `h` (go home) shortcuts
- **Always In Sync**: Auto-updates whenever bookmarks change
- **Per-Group Support**: Different bookmark groups generate different global keymaps

---

### 4. Enhanced Bookmark Group Management

All existing group management features are retained and enhanced:

- **Switch Group (FZF)**: `b` `g` `s` - Fuzzy search through groups
- **Switch Group (Quick)**: `b` `g` `g` - Numbered menu for fast switching
- **Create Group**: `b` `g` `c` - Create a new bookmark group
- **Delete Group**: `b` `g` `d` - Delete an existing group (not active)

---

## Complete Keybinding Reference

### Bookmark Management
| Keys | Action | Description |
|------|--------|-------------|
| `b` `a` `c` | Add Current | Bookmark current directory |
| `b` `a` `s` | Add Selection | Bookmark selected directory |
| `b` `a` `f` | Add from Search | Search filesystem and bookmark |
| `b` `a` `g` | Add Global | Bookmark with global scope keybinding |

### Jump/Navigate
| Keys | Action | Description |
|------|--------|-------------|
| `b` `k` `s` | Jump by Key | Select bookmark from key menu |
| `b` `f` `s` | Jump by FZF | Select bookmark via fuzzy search |
| `j` `b` | Quick Jump | Alternative FZF jump shortcut |

### Rename/Modify
| Keys | Action | Description |
|------|--------|-------------|
| `b` `k` `r` | Rename Tag | Rename bookmark name (keep key) |
| `b` `k` `k` | Reassign Key | Change bookmark shortcut (keep name) |
| `b` `f` `r` | Rename Tag (FZF) | Rename via FZF (keep key) |
| `b` `f` `k` | Reassign Key (FZF) | Change shortcut via FZF (keep name) |

### Delete
| Keys | Action | Description |
|------|--------|-------------|
| `b` `k` `d` | Delete by Key | Delete bookmark from key menu |
| `b` `f` `d` | Delete by FZF | Delete bookmark via fuzzy search |

### Group Management
| Keys | Action | Description |
|------|--------|-------------|
| `b` `g` `s` | Switch (FZF) | Switch group with fuzzy search |
| `b` `g` `g` | Switch (Quick) | Switch group with numbered menu |
| `b` `g` `c` | Create | Create new bookmark group |
| `b` `g` `d` | Delete | Delete existing group |

### Global Keymap
| Keys | Action | Description |
|------|--------|-------------|
| `b` `G` `g` | Generate | Manually generate global keybindings file |

---

## Migration Notes

### From Original whoosh.yazi

If you're coming from the original whoosh plugin:

1. All existing bookmarks are compatible (same file format)
2. Bookmark files are now stored in `~/.config/yazi/.data/bookmarks/` by default
3. The `default` group file contains your main bookmarks
4. New features are opt-in (global keymap generation can be disabled)

### Backward Compatibility

All existing features work exactly as before:
- Local scope bookmarks (with `b` prefix) still work
- Bookmark file format is unchanged
- All original keybindings are preserved

---

## Tips & Best Practices

### Global Scope Keybindings

1. **Use Consistent Prefixes**: Group related bookmarks under common prefixes (e.g., `g` for "go to", `p` for "projects")
2. **Avoid Conflicts**: The plugin checks for conflicts with existing keybindings in `keymap.toml`
3. **Reload Yazi**: After generating global keybindings, reload Yazi for changes to take effect

### Bookmark Groups

1. **Organize by Context**: Create groups like "work", "personal", "media", etc.
2. **Quick Switch**: Use `b` `g` `g` for fast switching between your most-used groups
3. **Active Group Indicator**: The `*` marker shows your current active group

### Workflow Optimization

**Before** (6 keystrokes to jump to WezTerm config):
```
b → f → s → <search "wezterm"> → Enter
```

**After with Global Scope** (2 keystrokes):
```
g → w
```

That's a **67% reduction** in keystrokes for common navigation tasks!

---

## Troubleshooting

### Global Keybindings Not Working

1. Check that `bookmarks-global.toml` exists at `$YAZI_CONFIG_HOME/.data/bookmarks-global.toml`
2. Verify the `#:include` directive is in your `keymap.toml`
3. Reload Yazi after generating the file
4. Check for keybinding conflicts (the plugin will notify you)

### Auto-Generation Not Triggering

1. Verify `auto_generate_global_keymap = true` in your `init.lua` setup
2. Check that the bookmark was saved successfully (notification should appear)
3. Ensure the `.data` directory is writable

### Group Switching Issues

1. Make sure groups exist in `$YAZI_CONFIG_HOME/.data/bookmarks/`
2. Check that the `.active_group` file is being updated
3. Verify permissions on the bookmarks directory

---

## Technical Details

### File Structure
```
$YAZI_CONFIG_HOME/
├── .data/
│   ├── bookmarks/              # Bookmark group files
│   │   ├── default             # Default group
│   │   ├── work                # Work group
│   │   ├── personal            # Personal group
│   │   └── .active_group       # Tracks current active group
│   └── bookmarks-global.toml   # Auto-generated global keybindings
├── init.lua
├── keymap.toml
└── dev/
    └── fzf-bookmarks.yazi/
        └── main.lua
```

### Auto-Generation Triggers

The global keymap file is regenerated on these events:
- `action_save()` - Adding new bookmark
- `action_rename_tag()` - Renaming bookmark
- `action_reassign_key()` - Changing bookmark key
- `action_delete()` - Deleting bookmark
- `action_delete_multi()` - Batch deleting bookmarks

---

## Future Enhancements (Potential)

- [ ] Per-group global keymaps (multiple includes)
- [ ] Visual keymap conflict detector
- [ ] Bookmark import/export functionality
- [ ] Bookmark search by tag
- [ ] Nested bookmark groups

---

## Credits

Enhanced and maintained by: @theron (based on user requirements)
Original plugin: Based on whoosh.yazi bookmark system
Yazi framework: https://github.com/sxyazi/yazi
