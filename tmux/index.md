
# TMUX

General usage tips for the CLI tool `tmux`, which is usually preinstalled on Ubuntu. Otherwise, install with `$ sudo apt-get install tmux`.

You can use it to create and arrange complex panes of parallel terminals. You can also detach from a session, which returns you to the "host" but keeps that session alive in the background so that you can reattach later.

The devs really went nuts with the functionality – especially the hotkeys. Which is why I deem it necessary to create this personal custom tutorial tailored to my needs.

Other interesting reads:

* [Tactical tmux: The 10 Most Important Commands](https://danielmiessler.com/p/tmux/)


　​

## Basic usage

```bash
# most basic usage: open new unnamed session.
[host]$ tmux

# inside session:
[tmux]$ echo "You are inside the session now."
[tmux]$ echo "I will mark that with ← this [tmux] prefix in this documentation."
```

You are now inside a new session.

All further TMUX-specific commands are prefixed by <kbd>CTRL</kbd>+<kbd>B</kbd>. For instance, in order to detach, you press <kbd>CTRL</kbd>+<kbd>B</kbd> together, followed by <kbd>D</kbd> alone. This is henceforth expressed as `CTRL+B—D` or `^B—D`.

You detach from a session in order to keep it alive. In contrast, exiting a session with `exit` kills that session.


　​

### Further subcommands

These work on the host-level but also from within a session.

```bash
# open new named session.
[host]$ tmux new -s batman

# detach from current session.
[tmux]$ tmux detach

# list sessions. Unnamed sessions use numeric indices.
[host]$ tmux ls
[tmux]$ tmux ls

# reattach to session.
[host]$ tmux attach -t batman
[host]$ tmux attach -t 2

# kill a session.
[host]$ tmux kill-session -t batman

# kill all sessions.
[host]$ tmux kill-server
[host]$ killall tmux
```


　​

### Structure

You can group terminals in various ways. Natively, TMUX uses the following structure:

```
Session
└── Window
    └── Pane
```

Creating a new session automatically creates one window and one pane. (Compare with "Hotkeys" section for more info on window and pane management).

After creating and renaming some resources, the bottom status bar looks like this, for example:

```
[batman] 0:bash  1:game* 2:logtailing-
```

That is the session "batman" with three windows. The asterisk marks that we are currently in the "game" window.

When toggling the layout, you cycle through predefined layouts. I.e. when there are only two panes, you toggle between vertical and horizontal separation.


　​

## Hotkeys

> **Note:** <br />
> Some hotkeys use characters that are not easily available on non-american keyboard layouts. <br />
> For example, `^B—$` translates to `^B—SHIFT+4` on some keyboard layouts.


　​

Hotkey | Description | Context
-------|-------------|--------
`^B—?` | "Get help" … It just opens the key bindings config file. |
`^B—S` | Browse sessions. |
`^B—D` | Detach from session, return to host. |
`^B—$` | Rename current session. |
`^C` <br /> `Q` | Abort action. | I.e. renaming, scroll mode.
`^B—[` | Enter scroll mode. |
`^B—T` | Display time. (WTF? useless garbage) |
||
**Windows** ||
`^B—W` | Browse windows. |
`^B—C` | Create new window. |
? | Delete window. |
`^B—,` | Rename current window. |
`^B—n` | Go to next window. |
`^B—p` | Go to previous window. |
`^B—0` | Jump to window with index `0`. (0-9) |
||
**Panes** ||
`^B—%` | Create new pane: split current pane vertically. |
`^B—"` | Create new pane: split current pane horizontally. |
`^B—O` | Toggle-browse through panes of current window. |
`^B—Q` | Show pane indices. |
`^B—}` | Swap with next pane. |
`^B—{` | Swap with previous pane. |
`^B—X` | Kill current pane, confirm. |
`X` | Kill selected pane, confirm. <br /> Functionally, you can also do that for windows. <br /> But that breaks the GUI. I advise against it. | Browse windows
`^B—!` | Outsource current pane as new window. |
`^B—SPACE` | Toggle layout. |
`^B—ESC+1` | Specifically select predefined layout #1. (1-5) |


　​

## Scrolling

Scrolling is a bit wonky. It becomes more convenient if you have such a `~/.tmux.conf` file with the following content. Set it up, kill all tmux sessions and try again.

```
set -g mouse on
# sane scrolling:
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'copy-mode -e; send-keys -M'"

# use SHIFT+clicks now to select and copy text.

```

> **Note:** <br />
> You are in "scroll mode" if an indicator such as `[60/211]` appears in the top right corner.

