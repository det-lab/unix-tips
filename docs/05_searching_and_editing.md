# Searching and Editing from the Shell
Here is where things start to get more complex. Unix provides several powerful tools for searching and manipulating files directly from the command line. Whether you're hunting down a misplaced file or editing configuration files in bulk, these commands are useful for saving a lot of time and effort.
## Searching Filenames
### `find`
The `find` command recursively searches for files by name, type, size, or modification time. By default, the command prints the names of the files for which your entire expression is `true`, unless your expression contains an action besides `-prune` or `-quit`. 

`find` searches your directory tree rooted from a given starting point, evaluating your entire expression from left to right until the outcome is determined as `True` or `False`, at which point `find` moves on to the next file (unless using the `-quit` argument). Without a specified search root, the current directory `.` is assumed. Also, if a starting point argument would normally begin with `-`, `find` would try to treat it instead as an expression. For this reason, it is generally safer to prefix wildcard or dubius path names with either `./`, or to use absolute path names starting with `/`.

The expressions used to select files are comprised of one or more of the following four **primaries**.

#### 1. `options`:   Affects overall operation rather than the processing of a specific file.
Most common `find` options:

* `-maxdepth N`
    * Limits how many subdirectories deep your expression will search starting from the search root.
```bash
find . -maxdepth 1 -name "*.log" # Looks for .log files in the current directory
```
* `-mindepth N`
    * Skips a number of directory levels before applying tests.
```bash
find . -mindepth 2 -type f # Skips top and first subdirectory to find files
```
* `-depth`
    * Processes directory contents **before** directory itself. 
```bash
find . -depth -name ".txt" -delete # Removes .txt files from directory before directory is handled
```
* `-xdev`/`-mount`
    * Prevents `find` from crossing into other mounted filesystems such as partitions or mounted drives.
```bash
find / -xdev -name "*.conf" # Searches only root, skipping /mnt, /boot, and similar if mounted separately
```
#### 2. `tests`:     Returns a true or false value depending on the file's attributes.
>`N` can be `+N`, `-N`, or just `N`

Most common `find` tests:
* `-name PATTERN`/`-iname PATTERN`
   * Matches files by name. `-iname` is case-insensitive.
   * Wildcards like `*` and `?` are supported.
```bash
find . -name "*.txt"   # Find all .txt files
find . -iname "readme" # Find all case-insensitive files that start with "readme"
```
* `-type`
    * Matches by file type:
       * `f`: Regular file
       * `d`: Directory
       * `l`: Symbolic link
```bash
find . -type f        # Find regular files
find /testdir -type d # Find directories under /testdir
```
* `-mtime N`/`-mmin N`
    * Matches files by modification time. .
    * `-mtime` uses days, `-mmin` uses minutes.
```bash
find . -mtime -1 # Find files modified in the last day
find . -mmin +60 # Find files modified more than an hour ago
```
* `-size N[b|c|k|M|G]`
    * Matches files by size.
    * `b`: 512-byte blocks, `c`: bytes, `k`: Kb, `M`: Mb, `G`: Gb
```bash
find . -size +10M  # Find files larger than 10 Mb
find . -size -100K # Find files smaller than 100 Kb
find . -size 3b    # Find files that are exactly 3 blocks
```
* `-user NAME`/`-group NAME`
    * Find files owned by a specific user or group.
```bash
find /home -user admin # Find files owned by user named admin
find . -group www-data # Find files owned by www-data group
```
> `-nouser` and `-nogroup` can be used to find files with no associated user or group. Often used for deleted accounts. 
* `-empty`
    * Match empty files or directories
```bash
find . -type f -empty # Find empty files
find . -type d -empty # Find empty directories
```
* `-path`/`-wholename`
    * Match full relative path (from search root)
```bash
find . -path "./src/*.txt" # Find all .txt files in the src folder
```
* `-newer FILE`/`-anewer FILE`/`-cnewer FILE`
    * Match files based on their modified/access/change times respectively
```bash
find . -newer reference.txt # Find files which have been modified more recently than reference.txt
```
* `-readable`/`-writable`/`-executable`
    * Checks the accessibilities for the current user.
```bash
find . -type f -executable # Find executable files
```
#### 3. `actions`:   Have side effects and returns a true or false value.
Most common `find` actions:

* `-print`
    * This is a default action which does not need to be specified. `-print` prints the patch of each file which matches the rest of the expression.
* `-fprint FILE`
    * This prints the entire file name into a file, `FILE`, followed by a newline. This will also create the `FILE` if it does not already exist.
```bash
find . -name ".txt" -fprint "save_txt.txt" # Saves the path for every .txt file to save_txt.txt 
```
* `-delete`
    * Deletes each file which matches the expression: **Use with caution.** This command is often combined with `-type` to avoid deleting directories or special files by accident.
```bash
find . name "*.tmp" -type f -delete # Deletes all .tmp files
```
* `-exec COMMAND {} \;`
    * Runs a command for each matching file. The shell substitutes `{}` with the full path to the file when running. `\;` then terminates the command.
    * You can also replace `\;` with `+` to have the command run **once** with multiple file arguments instead of once **per file**.
```bash
find . -name "*.txt" -exec gzip {} \; # Compresses each .txt file
find . -name "*.log" -exec rm {} +    # Deletes all .log files in batches
```
* `-prune`
    * Stops `find` from descending into matching directories. This action is common for skipping paths.
```bash
find . -path "./.git" -prune -o -name "*.py" -print # Skips .git, listing Python files elsewhere.
```
* `-quit`
    * Stops after the first match. Useful for fast existence checks.

Some less common `actions` that are still useful in scripts include:

* `-print0` and `-fprint0 FILE`
    * Prints filenames with a null character instead of a newline. This action is ideal for filenames which include spaces.
* `-ls`
    * Prints `ls -dils` style info for each file.
* `-ok COMMAND ;`
    * Like `-exec`, but prompts for confirmation before running each command.
* `-execdir`, `-okdir`
    * Like `-exec`, but runs the command in the directory of the matching file instead of where `find` was run. Useful when the command depends on relative paths.
#### 4. `operators`: Connects the other arguments and affect when and whether they are evaluated.
`find` allows for you to combine multiple tests using **logical operators** such as **AND**, **OR**, and **NOT**, along with grouping using parentheses. These operators can be very powerful for creating more precise search expressions:
* `-and`/`-a` (or a space)
    * All expressions must be true. This is the defualt behavior of `find`: listing conditions will automatically use "and", but it can be writen explicitely with `-and`.
```bash
find . -type f -name "*.sh"
# Equivalent to:
find . -type f -and -name "*.sh"
# Equivalent to:
find . -type f -a -name "*.sh"
# All three will find .sh files
```
* `-or`/`-o`
    * Matches if either condition is true.
```bash
find . -name "*.jpg" -or -name "*.png" # Prints .jpg and .png files
```
* `-not`/`!`
    * Inverts a test
```bash
find . -type f -not -name "*.txt"
# or:
find . -type f ! -name "*.txt"
# Prints all regular files that are not .txt
```