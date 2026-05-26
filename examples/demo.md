---
title: "Terminal Productivity Tips"
author: "Your Name"
options:
    h1_slide_titles: true
---

# Terminal Productivity Tips

<!-- speaker_note: |
  Welcome everyone. Today we're going to cover practical terminal tips that will speed up your daily workflow.

  These are shell-agnostic and work in bash, zsh, and fish.

  [1 minute]
-->

*A practical guide to working faster on the command line*

Your Name


<!-- end_slide -->

# Why the Terminal?

<!-- speaker_note: |
  Ask the audience: how many of you use the terminal daily?

  The terminal is the most powerful interface once you know the basics. No mouse needed, composable commands, scriptable.

  Key message: invest 10 minutes learning a few tricks, save hours every week.

  [2 minutes]
-->

**Speed** — no mouse, no context switching

**Composability** — pipes connect everything

**Scriptability** — automate once, run forever

**Universality** — works on any server, any OS

<!-- pause -->

> "The terminal is the ultimate IDE." — every senior engineer


<!-- end_slide -->

# Navigating Directories Efficiently

<!-- speaker_note: |
  Most people cd into a directory, ls, then cd deeper. There are faster ways.

  autojump/zoxide learn your habits — you type 'j proj' and it takes you to /home/you/projects.

  The cdargs/backward feature with pushd/popd is built-in to bash/zsh and often overlooked.

  [3 minutes]
  [1 pause]
-->

### Built-in tricks

| Command | What it does |
|---|---|
| `cd -` | Go back to previous directory |
| `pushd /tmp` | Jump somewhere, save current spot |
| `popd` | Return to saved spot |

<!-- pause -->

### Smart jump tools

- **zoxide** — learns your habits: `z proj` jumps to your project
- **autojump** — similar, uses frequency

No more `cd ../../../..`


<!-- end_slide -->

# Search with ripgrep

<!-- speaker_note: |
  grep is powerful but slow on large trees. ripgrep is 10x faster and has sane defaults.

  It respects .gitignore by default, has colorized output, and uses smarter regex.

  [1 minute]
-->

### Instead of `grep -r`

```bash +exec
rg "TODO" --type rust
```


<!-- end_slide -->

# Search with fd

<!-- speaker_note: |
  fd is to find what ripgrep is to grep. Faster syntax, colorized output, respects .gitignore.

  [1 minute]
-->

### Instead of `find -name`

```bash +exec
fd ".md"
```


<!-- end_slide -->

# Top Memory Consumers

<!-- speaker_note: |
  htop is great but not always installed. ps with --sort flags works everywhere.

  This shows the top 5 memory-consuming processes.

  [1 minute]
-->

```bash +exec
ps aux --sort=-%mem | head -5
```


<!-- end_slide -->

# System Uptime

<!-- speaker_note: |
  Quick check on how long the system has been running and current load.

  [30 seconds]
-->

```bash +exec
uptime
```


<!-- end_slide -->

# Brace Expansion Demo

<!-- speaker_note: |
  Brace expansion is one of those features people don't know about but use constantly once they learn it.

  It generates arbitrary strings from a compact pattern.

  [1 minute]
-->

```bash +exec
echo {1..5}
```


<!-- end_slide -->

# Instant File Backup

<!-- speaker_note: |
  Brace expansion works for file operations too. Appending .bak to a file is a one-liner.

  [30 seconds]
-->

```bash +exec
cp -v presenterm/SKILL.md{,.bak}
```


<!-- end_slide -->

# Disk Usage at a Glance

<!-- speaker_note: |
  df -h shows filesystem usage in human-readable format. du -sh * shows per-directory sizes.

  ncdu is even better — interactive TUI with keyboard navigation and delete support.

  [1 minute]
-->

```bash +exec
df -h /
```


<!-- end_slide -->

# Summary

<!-- speaker_note: |
  Recap the main points. Emphasize that these aren't esoteric tricks — they're daily-use commands.

  The biggest time-savers: ripgrep for search, zoxide for navigation, brace expansion for fileops.

  Challenge the audience: pick ONE of these and use it tomorrow.

  [1 minute]
-->

| Tool | Replaces | Why |
|---|---|---|
| `rg` | `grep -r` | 10x faster, .gitignore-aware |
| `fd` | `find` | Simpler syntax, colorized |
| `zoxide` | `cd` + guesses | Learns your projects |
| `du -sh *` | clicking around | Immediate disk insight |

**Pick one. Use it tomorrow. Save hours.**


<!-- end_slide -->

# Q&A

<!-- speaker_note: |
  Open the floor for questions.

  Have these backup topics ready if no one asks:
  - tmux vs screen
  - fzf for fuzzy searching history
  - jq for JSON processing in the terminal

  [Remaining time]
-->

*Questions?*

**Resources:**
- [presenterm](https://github.com/mfontanini/presenterm) — this presentation tool
- [ripgrep](https://github.com/BurntSushi/ripgrep)
- [zoxide](https://github.com/ajeetdsouza/zoxide)

*Thank you!*


<!-- end_slide -->
