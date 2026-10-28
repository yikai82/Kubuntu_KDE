# tmux Cheatsheet
**Yi-Kai's quick reference — Octorber 2026**

tmux stands for "terminal multiplexer." It allows you to run multiple terminal sessions by creating separate persistent sessions. You can disconnect from a session without stopping the programs running inside it, then reattach when needed.

> [!NOTE]  
> 1. Fix the mouse scrolling issue in tmux first 
>       - By default, scrolling with the mouse wheel types strange escape codes. Enable mouse support to fix this. It is a one-time setup, 
>       ```bash
>       echo "set -g mouse on" >> ~/.tmux.conf
>       tmux source-file ~/.tmux.conf
>       # The `-g` flag makes it global across all sessions and windows.
>       ```
> 2. To exit the session, use  `Ctrl+B` then `D`
> 



---
## 📋 Sessions

| Action | Command |
|---|---|
| New named session | `tmux new -s session_name` |
| List all sessions | `tmux ls` |
| Switch sessions visually | `Ctrl+B` then `S` |
| Next session | `Ctrl+B` then `)` |
| Previous session | `Ctrl+B` then `(` |
| Switch to session by name | `tmux switch -t session_name` |
| Detach from session | `Ctrl+B` then `D` |
| Reattach to session | `tmux attach -t session_name` |


---
## 📋 Copy → 📄 Paste


### Option 1 — Shift+drag (easiest)
Hold `Shift` while clicking and dragging — bypasses tmux and lets your terminal handle copy normally.

### Option 2 — tmux copy mode

| Action | Keys |
|---|---|
| Enter copy mode | `Ctrl+B [` |
| Start selection | `Space`, then move cursor |
| Copy selection | `Enter` |
| Paste | `Ctrl+B ]` |

**Tip:** Most people just use **Shift+drag** for copy — way less friction.

---
## 🪟 Windows (within a session)

| Action | Command |
|---|---|
| New window | `Ctrl+B` then `C` |
| Next window | `Ctrl+B` then `N` |
| Previous window | `Ctrl+B` then `P` |
| Jump to window by number | `Ctrl+B` then `0`, `1`, `2`... |
| See all windows visually | `Ctrl+B` then `W` |

