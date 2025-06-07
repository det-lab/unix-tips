# Navigation
Before you can manage files and run scripts, it's necessary to understand and be comfortable with moving around your system from inside the shell. The Unix shell provides flexible commands for exploring the directory structure. This section will cover tips and common patterns for navigation that will save you time and help prevent mistakes.

## Basic Commands
* `pwd`: Print Working Directory. This command gives a print out of the path to reach the directory you're currently working from. For instance, if you're opening Linux, `pwd` might return a path like: `/home/userName`

* `ls`: List. This command shows the contents of the current directory. 

* `cd <directory>`: Change Directory. This command allows you to move into a different folder.

> You can also use `ls <directory>` or `ls <path>` to show the contents of a folder other than the one you're currently in. 

## Useful Shortcuts
* `~` represents your home directory path, so `cd ~` will change your directory to the top of your folder tree and `ls ~` will show you same folder's contents.

* `cd -` will return you to the previous directory you were in.

* `.` refers to the current directory.
 
* `..` refers to the parent directory, so `cd ..` will move you into the parent directory and `ls ..` will show the contents of the parent directory.

## Tab Completion
You can start typing a directory or file name and press `Tab` to auto-complete. If multiple matches exist, pressing `Tab` twice will show you the options. This also works inside of different directories, meaning you can use `Tab` for each directory to shorten how much is needed to type to navigate deeper into a path. For instance:
```bash
cd Doc<tab>/Git<tab>/
```
would move you into the `Documents/Github` folder. 
> Tab completion works slightly differently in different shells. `Powershell` and `Fish` are not case sensitive, while `Bash` and `Tcsh` are.

## Listing Files More Effectively
There are several options that can be added after `ls` to modify its behavior. For a complete list, you can use `ls --help`. It's also important to note that it is possible to use different modifiers together for more comprehensive results. 

For instance, `-a` will show all files, including hidden ones such as dotfiles like `.gitignore`, while `-A` will show almost all files, ignoring `.` and `..` entries. `-l` will use a long listing format, `-h` will use a human readable format, `-r` will reverse the result order, and `-t` will sort by modified time. Stacking these options together allows for searches such as:
```bash
ls -lh # Long listing format with human-readable file sizes
ls -ltr # Long listing format, sort by modified time, oldest first
ls -Alh # Show almost all files, long listing format, human readable
```
## Using `tree` for a Visual Directory Structure
The `tree` command visually maps directory structures, making it especially helpful when you need to understand the organization of nested directories, showing you directories without requiring you to manually navigate through each one. 

By default, `tree` is not installed, but it can be installed with the commands:
```bash
sudo yum install tree
```
or
```bash
sudo dnf install tree
```
> For RHEL, CentOS, and Fedora Linux.
```bash
sudo apt-get install tree
```
> For Debian, Mint, and Ubuntu Linux.
```bash
brew install tree
```
> For Apple OS X.
### Basic Syntax of Tree Command
Just entering `tree` by itself will output your entire file structure starting from your current directory, making it not very useful without applying certain condtions. The basic command syntax is to use `tree [options]` for more precise usage. As before, you can enter `tree --help` in order to see a list of all options. 

A short list of options you might use more often include:

* `-a` or `--all`: Includes hidden files and directories in the tree.

* `-d` or `--dirs-only`: List directories only.

* `-f` or `--full-path`: Prints the full path prefix for each file.

* `-p` or `--prune`: Omits specified directory from the tree.

* `--filelimit #`: Doesn't descend directories that contain more than # entries.

* `-s`: Prints the size of each file along with the name.

* `-L #`: Sets a maximum depth, #, of the directory tree to display.

As before, these can also be mixed and matched. For instance, if you only wanted to see directories to a depth of 3, and  you wanted to see the path for each file, you could run:
```bash
tree -dpL 3
```

---

Now that you're familiar with moving around and inspecting the filesystem, let's look at how you can save time using **command history** and quick recall tools. [Click here](03_command_history.md) to continue on to the next section.