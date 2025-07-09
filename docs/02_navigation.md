# Navigation
Before you can manage files and run scripts, it's necessary to understand and be comfortable with navigating your file directory from inside the shell. This section will cover tips and common patterns for navigation that will save you time and help prevent mistakes.

## Basic Commands
### `pwd` 
This command stands for 'Print Working Directory'. This command gives a print out of the path to reach the directory you're currently working from. For instance, if you're opening Linux, `pwd` might return a path like: `/home/userName`

### `ls`
List. This command shows the contents of the current directory. 

### `cd <directory>`
Change Directory. This command allows you to move into a different folder.

>NOTE: You can also use `ls <directory>` or `ls <path>` to show the contents of a folder other than the one you're currently in. 

## Useful Shortcuts
* `~` represents your home directory path, so `cd ~` will change your directory to the top of your folder tree and `ls ~` will show you same folder's contents.

* `cd -` will return you to the previous directory you were in.

* `.` refers to the current directory.
 
* `..` refers to the parent directory, so `cd ..` will move you into the parent directory and `ls ..` will show the contents of the parent directory.

## `--help`
Most commands will provide you with a quick guide if you type the command + `--help`.

## Tab Completion
You can start typing a directory or file name and press `Tab` to auto-complete. If multiple matches exist, pressing `Tab` twice will show you the options. This also works inside of different directories, meaning you can use `Tab` for each directory to shorten how much is needed to type to navigate deeper into a path. For instance:
```bash
cd Doc<tab>/Git<tab>/
```
would move you into the `Documents/Github` folder (assuming you don't have other objects that start with the same three letters in the same folder). 
>NOTE: Tab completion works slightly differently in different shells. `Powershell` and `Fish` are not case sensitive, while `Bash` and `Tcsh` are.

## Stacking options
Most commands have a list of options that can be used to modify its results in specific ways. In most cases, it is possible to stack options so that they are all included in the execution of the command. 

As an example, there are several options that can be added after `ls` to modify its behavior. `-a` will show *all* files, while `-A` will show *almost* all files, ignoring files and folders that begin with `.` and `..`. `-l` will use a long listing format, `-h` will use a human readable format, `-r` will reverse the result order, and `-t` will sort by modified time. Stacking these options together allows for more complex list result searches such as:
```bash
ls -lh # Long listing format with human-readable file sizes
ls -ltr # Long listing format, sort by modified time, oldest first
ls -Alh # Show almost all files, long listing format, human readable
```
It is also allowed to list the commands one after the other, such as `ls -l -t -r`.
## Using `tree`
The `tree` command visually maps directory structures, making it especially helpful when you need to understand the organization of nested directories. It prints out a representation of your directories where the indentation level indicates subfolders. 

By default, `tree` is not installed. For RHEL, CentOS, and Fedora Linux, it can be installed with the commands:
```bash
sudo yum install tree
```
or
```bash
sudo dnf install tree
```
For Debian, Mint, and Ubuntu Linux:
```bash
sudo apt-get install tree
```
And for Apple OS X, it can be installed with:
```bash
brew install tree
```
Just entering `tree` by itself will output your entire file structure starting from your current directory. Depending on the number of subfolders, this might result in a cluttered mess unless you apply certain options. 

The basic command format for `tree` follows:
```bash
tree [options]
```

Some of the more common options for `tree` include:

### `-a`/`--all`
These options include hidden files and directories in the results.

### `-d`/`--dirs-only`
List directories only.

### `-f`/`--full-path`
Prints the full path prefix for each file.

### `-p`/`--prune`
Omits specified directory from the tree.

### `--filelimit #`
Prevents `tree` from descending into directories which contain more than `#` entries.

### `-s`
Prints the size of each file along with the name.

### `-L #` 
Sets a maximum depth, #, of the directory tree to display.

As before, these options can be stacked together for more precise results. For instance, if you only wanted to visualize directories up to 3 folders deep while also wanting to see the path for each file, you could run:
```bash
tree -dpL 3
```

---

Now that you're familiar with moving around and inspecting the filesystem, let's look at how you can save time using **command history** and quick recall tools. [Click here](03_command_history.md) to continue on to the next section.