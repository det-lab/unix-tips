# Command History and Quick Recall Tools
Typing the same long commands over and over can get tedious and error prone. Fortunately, Unix shells keep a **command history** that lets you easily recall, repeat, and modify previous commands. This section covers essential shortcuts and tools to speed up your workflow.

## Basic History Navigation
* Press the up arrow to scroll through previous commands.

* Press the down arrow to move forward again.

You can also view a full numbered history of your previous commands with:
```bash
history
```

## Reverse search
Unix supports a reverse search, allowing you to find previous commands by typing in a segment of them. From the shell, press `CTRL+R` to begin a reverse search. You can then start typing a previous command and the shell will search backward through your history, autocompleting your search on the command line.

* Keep pressing `CTRL+R` to cycle through more matches.

* Press `CTRL+S` to search forward

* Press `Enter` to run the found command.

* Press `Esc` or `CTRL+G` to cancel the search.

# Using `Fish`
The Fish shell offers even more intuitive and powerful history navigation tools than supported by a base Unix installation. You can download Fish by following [this link to the official site](https://fishshell.com/), or in the shell you can run the command:
```bash
sudo apt install fish
```
Once installed, you only need to enter `fish` on the command line to get started. 

The Fish shell allows you to begin typing any part of a previous command, and it will automatically suggest matching commands from your history. You can then press the **up arrow** or **down arrow** to search through your matching command history. Then, press the **right arrow** to accept the suggestion, and **Enter** to run the command.

Fish also includes advanced history commands:

* `history search <pattern>` shows matching commands. For instance, `history search cd` would show you each command you've used to change directories in your saved history.

* `history delete --prefix <pattern>` deletes entries from your history that start with a given prefix.

>NOTE:  Note: Fish keeps a separate history **per session** and **per working directory**, making it especially useful for project-based work.

Fish also helpfully uses syntax highlighting as you type. By default, Fish will color invalid commands red as you write them, and underline valid file paths.

## View and Edit Commands Before Running
You can use `fc` (fix command) to open your previous command in your default editor, such as `nano`, `vim`, or `emacs`. This can be useful for making edits to long or complex commands before re-running them.

---

Now that you can navigate and reuse your command history efficiently, we'll move on to **file operations**: copying, moving, renaming, and deleting files using the shell. [Click here to continue on to the next section.](04_file_operations.md)