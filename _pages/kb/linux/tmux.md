---
layout: page
title: TMux
permalink: /kb/linux/tmux
---

### Tmux - a terminal multiplexer - https://tmux.github.io/

#### Sessions

- `tmux ls` – List running sessions.
- `tmux new -s name` – Create a new session with the name name.
- `tmux a -t name` – Attach to an existing session with the name name.
- `Ctrl+b d` – Detach from the current session.
- `Ctrl+b D` - Note capital `D`. Detach another user from this session.
- `exit` – Close(end) the current session (terminate the bash instance).

#### Windows

- `Ctrl+b w` – List open windows.
- `Ctrl+b c` – Create a new window.
- `exit` – Close(end) the current window (terminate the bash instance).
- `Ctrl+b n` – Go to the next window.
- `Ctrl+b p` – Go to the previous window.
- `Ctrl+b PageUp and PageDown` – Scroll up and down in the window(exit with q).
- `Ctrl+b %` – Split window vertically(exit bash instance to close split).
- `Ctrl+b “` – Split window horizontally(exit bash instance to close split).
- `Ctrl+b Up/Down/Left/Right arrow` – Move the focus into next split.
- `Hold Ctrl+b and press Up/Down/Left/Right arrow` – Resize current split.
- `Ctrl+b &` – Kill current window.
- `Ctrl+b ,` – Rename current window.
- `set-option -g history-limit 3000` – Set scroll buffer size.
