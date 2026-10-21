[README.md](https://github.com/user-attachments/files/33766682/README.md)
# ghostty-dev-setup

A colorful, fast, **plugin-manager-free** terminal setup for macOS developers:
[Ghostty](https://ghostty.org) + zsh + [Starship](https://starship.rs) + tmux + a modern git workflow, all themed with [Catppuccin Mocha](https://catppuccin.com).

<!-- Add a screenshot: drop one in docs/ and uncomment
![screenshot](docs/screenshot.png)
-->

- 🎨 **One theme everywhere**: Ghostty, the prompt, tmux, fzf, bat and git diffs all match
- ⚡ **Fast**: about 0.15s shell startup
- 🔒 **Nothing hidden**: everything comes from official Homebrew packages, there are no `curl | sh` installers and no plugin managers pulling code from random repos, and it never uses `sudo`
- ↩️ **Reversible**: every file it replaces is backed up and `./uninstall.sh` restores it
- 🧩 **Easy to tweak**: plain config files, plus a `~/.zshrc.local` for your own settings

---

## Quick start

```sh
git clone https://github.com/PacketCowboy/ghostty-dev-setup.git ~/ghostty-dev-setup
cd ~/ghostty-dev-setup
./install.sh --dry-run    # see exactly what it will do
./install.sh              # do it
```

Then **quit Ghostty completely (Cmd+Q) and reopen it**.

**Requirements:** macOS, [Homebrew](https://brew.sh), and git (`xcode-select --install`).

| Option | Effect |
|---|---|
| `--dry-run` | Print every action, change nothing |
| `--copy` | Copy configs instead of symlinking them (use this if you'll delete or move the repo folder) |
| `--no-brew` | Don't install packages |
| `--no-omz` | Don't install Oh My Zsh |
| `-y` | Don't ask for confirmation |

---

## What it installs

**Packages** (see [`Brewfile`](Brewfile)): Ghostty, JetBrains Mono and Meslo Nerd Fonts, starship, tmux, zsh-autosuggestions, zsh-syntax-highlighting, eza, bat, fd, fzf, zoxide, fastfetch, git, git-delta, lazygit, gh.
It also installs **Oh My Zsh**, cloned from its official repo with `git clone`, only if you don't already have it.

**Configs** (symlinked from this repo by default):

| Your file | From |
|---|---|
| `~/.zshrc` | [`config/zsh/zshrc`](config/zsh/zshrc) |
| `~/.config/starship.toml` | [`config/starship/starship.toml`](config/starship/starship.toml) |
| `~/.config/tmux/tmux.conf` | [`config/tmux/tmux.conf`](config/tmux/tmux.conf) |
| `~/Library/Application Support/com.mitchellh.ghostty/config.ghostty` | [`config/ghostty/config.ghostty`](config/ghostty/config.ghostty) |
| `~/.config/git/dev.gitconfig` | [`config/git/dev.gitconfig`](config/git/dev.gitconfig) |

**Your `~/.gitconfig` is never replaced.** The installer adds a single `[include]` line pointing at `dev.gitconfig`, and your name, email and other settings stay exactly as they were.

It also creates `~/.zshrc.local` for your own settings if you don't have one, and makes your shell history readable only by you (`chmod 600 ~/.zsh_history`).

---

## Cheatsheet

### Ghostty
| Keys | Action |
|---|---|
| Ctrl+\` | Drop-down terminal from any app (needs Accessibility permission) |
| Cmd+D / Cmd+Shift+D | Split right / split down |
| Cmd+Opt+Arrows | Move between splits |
| Cmd+Shift+Enter | Zoom a split |
| Cmd+Shift+, | Reload config |

### Shell
| Command / Keys | Action |
|---|---|
| Ctrl+R | Fuzzy search history |
| Ctrl+T | Fuzzy find a file (with preview) |
| Alt+C | Fuzzy jump to a folder |
| Ctrl+Space | Accept the gray suggestion |
| Esc Esc | Put `sudo` in front of the last command |
| `cd foo` | zoxide: jump to the best-matching folder you've visited; `cdi` = interactive |
| `ls` / `ll` / `lt` | eza with icons / long listing + git status / tree |
| `cat` | bat (syntax highlighted) |
| `mkcd dir` | Make a folder and cd into it |
| `reload` | Restart the shell |

### Git
| Command | Action |
|---|---|
| `lg` | lazygit (full-screen git UI) |
| `gcof` | Fuzzy-pick a branch and check it out |
| `gshow` | Browse commits with diff preview |
| `glo` | Pretty commit graph |
| `gs` | Short status |
| `gundo` | Undo last commit, keep changes |
| `gwip` | Quick "wip" commit (skips hooks; local use only) |
| `git amend` / `git last` / `git lg` | Amend without editing / show last commit / graph of all branches |
| + every [Oh My Zsh git alias](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git) | `gst`, `gco`, `gcb`, `gp`, `gl`, … |

Git defaults: diffs go through delta, `pull` rebases, the first `push` on a new branch sets up the remote, conflicts use `zdiff3`, branches are sorted by most recent, and `rerere` (reuse recorded conflict resolutions) is on.

### tmux (prefix = **Ctrl+a**)
| Keys | Action |
|---|---|
| `t` / `t name` | Attach to or create session "main" / "name" (`tl` list, `tk name` kill) |
| `prefix \|` or `prefix \` / `prefix -` | Split side by side / top and bottom |
| Alt+Arrows | Move between panes (no prefix) |
| Shift+←/→ | Previous / next window (no prefix) |
| `prefix h j k l` / `H J K L` | Move between / resize panes |
| `prefix c` / `prefix z` | New window / zoom pane |
| `prefix g` | lazygit popup |
| `prefix f` | Fuzzy session switcher |
| `prefix t` | Floating scratch shell |
| `prefix S` | New named session |
| `prefix Enter` | Copy mode (`v` select, `y` copy to the Mac clipboard) |
| `prefix r` | Reload config |

---

## Make it yours

| Want to… | Do this |
|---|---|
| Add PATHs, aliases, env vars | Put them in **`~/.zshrc.local`** (never overwritten) |
| Change the prompt style | `starship preset --list`, then `starship preset tokyo-night -o ~/.config/starship.toml` |
| Change Ghostty's theme | `ghostty +list-themes`, then edit `theme =` in `config/ghostty/config.ghostty` |
| Change the font | Edit `font-family`; list fonts with `ghostty +list-fonts` |
| Use Ctrl+b as tmux prefix | Edit the top of `config/tmux/tmux.conf` |
| Override a git default | Set it in your own `~/.gitconfig` (your settings win) |
| Track your changes | Fork this repo; with the default symlink mode your edits are already live in the repo |

> **Symlink mode** (default): your config files point into this folder, so edits here take effect right away and `git pull` brings in updates. Don't move or delete the folder; if you plan to, use `--copy`.

---

## Uninstall

```sh
./uninstall.sh            # restore from the latest backup
./uninstall.sh --dry-run  # preview
```

Backups live in `~/.ghostty-dev-setup-backups/<timestamp>/`. Homebrew packages, Oh My Zsh and `~/.zshrc.local` are left in place; the uninstaller prints the command to remove the packages.

---

## Security notes

- Every package comes from the official `homebrew/core` and `homebrew/cask` sources; no third-party sources (`brew tap`) are added.
- The installer never uses `sudo`, never pipes a download into a shell, and only writes inside your home folder.
- No tmux, zsh or vim plugin managers are used.
- Things worth knowing about:
  - **`copy-on-select = clipboard`** (Ghostty): highlighted text goes to the clipboard, which other apps can read. Remove the line if you handle secrets often.
  - **The drop-down terminal hotkey** needs macOS Accessibility permission. Don't grant it, or delete the `keybind = global:` line, if you don't want that.
  - **Shell history**: start a command with a space to keep it out of history (handy for anything containing a token).
  - **tmux `allow-passthrough on`**: lets programs inside tmux talk to Ghostty directly (needed for inline images). Set it to `off` if you don't need that.

---

## Layout

```
.
├── install.sh / uninstall.sh
├── Brewfile
└── config/
    ├── ghostty/config.ghostty
    ├── zsh/zshrc
    ├── starship/starship.toml
    ├── tmux/tmux.conf
    └── git/dev.gitconfig
```

## Credits

[Ghostty](https://ghostty.org) · [Starship](https://starship.rs) · [Catppuccin](https://catppuccin.com) · [Oh My Zsh](https://ohmyz.sh) · [Nerd Fonts](https://www.nerdfonts.com) · [eza](https://eza.rocks) · [bat](https://github.com/sharkdp/bat) · [fd](https://github.com/sharkdp/fd) · [fzf](https://github.com/junegunn/fzf) · [zoxide](https://github.com/ajeetdsouza/zoxide) · [delta](https://github.com/dandavison/delta) · [lazygit](https://github.com/jesseduffield/lazygit) · [tmux](https://github.com/tmux/tmux)

## License

[MIT](LICENSE)
