---
layout: page
ptitle: Terminal multiplexer
---

### Tmux - a terminal multiplexer - https://tmux.github.io/

#### Sessions
- `tmux ls` - List running sessions.
- `tmux new -s name` - Create a new session with the name name.
- `tmux a -t name` - Attach to an existing session with the name name.
- `Ctrl+b d` - Detach from the current session.
- `Ctrl+b D` - Note capital `D`. Detach another user from this session.
- `Ctrl+b $` - Rename current session.
- `exit` - Close(end) the current session (terminate the bash instance).

#### Windows
- `Ctrl+b w` - List open windows per all sessions.
- `Ctrl+b c` - Create a new window.
- `exit` - Close(end) the current window (terminate the bash instance).
- `Ctrl+b n` - Go to the next window.
- `Ctrl+b p` - Go to the previous window.
- `Ctrl+b &` - Kill current window.
- `Ctrl+b ,` - Rename current window.

#### Panes
- `Ctrl+b %` - Create pane by splitting window vertically(exit bash instance to close split).
- `Ctrl+b “` - Create pane by splitting window horizontally(exit bash instance to close split).
- `Ctrl+b Space` - Switches to next layout - Arrange panes in one of the five preset layouts: even-horizontal, even-vertical, main-horizontal, main-vertical, or tiled.
- `Ctrl+b x` - Kill the current pane.
- `Ctrl+b z` - Toggle current pane between zoomed (occupying the whole of the window) and unzoomed (its normal position in the layout).
- `Ctrl+b PageUp and PageDown` - Scroll up and down in the current window/pane(exit with q).
- `set-option -g history-limit 3000` - Set scroll buffer size.
- `Ctrl+b Up/Down/Left/Right arrow` - Move the focus into next pane.
- `Hold Ctrl+b and press Up/Down/Left/Right arrow` - Resize current pane.

#### Other
- `Ctrl+b : setw synchronize-panes on` - Broadcast input to all panes in window(toggle on/off).

```bash
# Run multiple commands in multiple panes
tmux new-session ping 192.168.1.1 \; split-window -h ping 8.8.8.8 \; split-window -v ping 192.168.0.1
tmux new-session ping 192.168.1.1 \; split-window -h ping 8.8.8.8 \; split-window -v ping 192.168.0.1 \; select-pane -t 0 \; split-window -v ping 192.168.2.4

# Attach to a session or (create session and attach to it) using a name
# Works great with interruptible ssh sessions.
ssh -t server-name tmux new-session -A -s session_name
```
