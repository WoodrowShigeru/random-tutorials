
# TMUX

General usage tips for the CLI tool `tmux`, which is usually preinstalled on Ubuntu. Otherwise, install with `$ sudo apt-get install tmux`.

You can use it to create and arrange complex panes of parallel terminals. You can also detach from a session, which returns you to the "host" but keeps that session alive in the background so that you can reattach later.

The devs really went nuts with the functionality – especially the hotkeys. Which is why I deem it necessary to create this personal custom tutorial tailored to my needs. As such, this documentation is just an excerpt. Look at `man tmux` for more info.

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

All further TMUX-specific commands are prefixed by <kbd>CTRL</kbd>+<kbd>b</kbd>. For instance, in order to detach, you press <kbd>CTRL</kbd>+<kbd>b</kbd> together, followed by <kbd>d</kbd> alone. This is henceforth expressed as `CTRL+b—d` or `^b—d`.

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

> **Note:** <br />
> When selecting a session, window or pane via either of the respective hotkeys – i.e. `^b—w` – be aware that you load that respective session into the current terminal. That can get confusing in case you're already using dedicated terminals per session: you didn't switch to the other terminal – you now have two terminals showing the same session. – There is no data loss. You can just switch back the same way.

When toggling the layout with `^b—space`, you cycle through predefined layouts. I.e. when there are only two panes, you toggle between vertical and horizontal separation.


　​

## Hotkeys

> **Notes:** <br />
> Some hotkeys use characters that are not easily available on non-american keyboard layouts. <br />
> For example, `^b—$` translates to `^b—SHIFT+4` on some keyboard layouts.
> ---
> The hotkeys are case-sensitive. For example, `^b—o` does something else than `^b—O`.


　​

Hotkey | Description | Context
-------|-------------|--------
`^b—?` | "Get help" … It just opens the key bindings config file. |
`^b—s` | Browse sessions. <br /> This is a tree view. |
`^b—d` | Detach from session, return to host. |
`^b—$` | Rename current session. |
`^c` <br /> `Q` | Abort action. | I.e. renaming, scroll mode.
`^b—[` | Enter scroll mode. (Or copy mode … I don't care) |
`^b—t` | Display time. (arguable usefulness) |
`right` | Expand tree node. | Tree view.
`left` | Collapse tree node. | Tree view.
||
**Windows** ||
`^b—w` | Browse windows. <br /> This is a tree view where you can even select panes. |
`^b—c` | Create new window. |
`^b—&` | Kill current window, confirm. |
`^b—,` | Rename current window. |
`^b—n` | Go to next window. |
`^b—p` | Go to previous window. |
`^b—0` <br /> … (0-9) | Jump to window with index `0`. |
||
**Panes** ||
`^b—%` | Create new pane: split current pane vertically. |
`^b—"` | Create new pane: split current pane horizontally. |
`^b—o` | Toggle-browse through panes of current window. |
`^b—q` | Show pane indices. |
`^b—}` | Swap with next pane. |
`^b—{` | Swap with previous pane. |
`^b—x` | Kill current pane, confirm. |
`x` | Kill selected pane, confirm. <br /> Functionally, you can also do that for windows. <br /> But that breaks the GUI. I advise against it. | Browse windows
`^b—!` | Outsource current pane as new window. |
`^b—space` | Toggle layout. |
`^b—esc+1` <br /> … (1-5) | Specifically select predefined layout #1. |
`^b—up` <br /> `^B—left` <br /> `^B—down` <br /> `^B—right` | Navigate panes. |
`^b—;` | Go to last used pane. |
`^b—z` | Zoom active pane. Or, as I like to call it, the "we hereby acknowledge that our select-copy-paste experience sucks, which is why we've had to create this workaround" mode. Zoomed panes are marked with a `Z` suffix in the status line. |
`^b—ALT+left` <br /> `^b—ALT+right` <br /> `^b—ALT+up` <br /> `^b—ALT+down` | Resize pane by 5 magic units. |


　​

## Scrolling

Scrolling is a bit wonky. It becomes more convenient if you have such a `~/.tmux.conf` file with the following content. Set it up, kill all tmux sessions and try again.

```bash
set -g mouse on
# sane scrolling:
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'copy-mode -e; send-keys -M'"

# use SHIFT+clicks now to select and copy text.
```

> **Note:** <br />
> You are in "scroll mode" if an indicator such as `[60/211]` appears in the top right corner.


　​

## Selecting text and copy-paste

* Very unintuitive!
* Beware that you have to distinguish between the system clipboard and the so-called "(tmux) paste buffer" – something else entirely!
	* The system clipboard is what most apps use: a global clipboard that is shared between the apps.
	* Tmux uses the so-called copy mode and scroll mode to copy text into the tmux paste buffer. An exclusive local clipboard.
* Because of ↑ our script (fair enough), we have to hold `SHIFT` while dragging. That is what we want, most of the time: copy into the system clipboard. — And also hold `SHIFT` again, when right-clicking for the context-menu, which has a "Copy" option.
* Selecting text without `SHIFT+click` will paste the text into the paste buffer.


　​

**Limitations:**

* Natively, if you have e.g. two, vertically split columns, the selected text will nonsensically span both columns. — Use the "zoom" mode to deal with that.
* Selecting with `SHIFT` is limited to what you can see in the current viewport. If you need more, you're gonna have to use the scroll mode.


　​

**Copying via the paste buffer:**

* `^b–[` to enter scroll mode.
* Use `up`, `down`, `PageUp` or `PageDown` to navigate to the beginning or end of what you want to copy.
* `CTRL+space` to start "select mode". Use those same navigational keys to complete your selection.
* People claim, confirm with `enter` key but that does not work for me. Use `ALT+w` instead.
* Now your text is in the paste buffer.
* \-\-\-
* Use the command `$ tmux save-buffer ./my-file` to overwritingly paste the text into a file … that you can open in your text editor. The `-a` flag appends instead.


　​

> **Note:** <br />
> The other way around is much easier: paste from system clipboard to tmux terminal via `CTRL+SHIFT+V`.

