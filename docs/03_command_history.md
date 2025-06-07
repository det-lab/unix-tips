# Command History and Quick Recall Tools
Typing the same long commands over and over can get tedious and error prone. Fortunately, Unix shells keep a **command history** that lets you easily recall, repeat, and modify previous commands. This section covers essential shortcuts and tools to speed up your workflow.

## Basic History Navigation
* Press the up arrow to scroll through previous commands.

* Press the down arrow to move forward again.

You can also view a full numbered history of your previous commands with:
```bash
history
```

## Search Through Command History
You can use reverse search to find previous commands be substring. From the shell, press `CTRL+R` to begin a reverse search. You can then start typing part of the command, and the shell will search backward through your history.

* Keep pressing `CTRL+R` to cycle through more matches.

* Press `Enter` to run the found command.

* Press `Esc` or `CTRL+G` to cancel the search.

> Tip: Use `CTRL+S` to search forward

## Repeat Previous Commands

* `!!`: Repeat the last command.

* `!n`: Run command number `n` from the output of `history`.

* `!string`: Run the **most recent** command starting with a chosen `string`.

* `!?string`: Run the most recent command **containing** a chosen `string`.

### Examples:
```bash
!42      # Repeats command number 42
!git     # Repeats the last command starting with "git"
!?status # Repeats the last command that had "status" anywhere
```

## Reuse Parts of Previous Commands
* `!$`: The last word of the previous command.

* `!*`: All **arguments** from the previous command.

* `^old^new`: Re-run the last command, replacing `old` with `new`.

### Examples:
```bash
mkdir project
cd !$    # Moves you into "project" folder

git commit -m "fiz bug"
^fiz^fix # Runs: git commit -m "fix bug"
```

## View and Edit Commands Before Running
You can use `fc` (fix command) to open your previous command in your default editor, such as `nano`, `vim`, or `emacs`. This can be useful for making edits to long or complex commands before re-running them.

---

Now that you can navigate and reuse your command history efficiently, we'll move on to **file operations**: copying, moving, renaming, and deleting files using the shell. [Click here] to continue on to the next section.