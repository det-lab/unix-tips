# Using `find` to Search Filenames
The `find` command recursively searches for files by name, type, size, or modification time. By default, the command prints the names of the files for which your entire expression is `true`.

`find` searches your directory tree rooted from a given starting point, evaluating the given expression from left to right until the outcome is determined as `True` or `False`, at which point `find` moves on to the next file. Without a specified search root, the current directory, `.`, is assumed. Also, if a starting point argument would normally begin with `-`, `find` would try to treat it instead as an expression. For this reason, it is generally safer to prefix any wildcard or dubious path names with `./`, or to use absolute paths such as `/home/user/Unix-Tips`.

The `find` command follows the format:
```bash
find [PATH] [EXPRESSION]
```
The `expressions` used to select files are comprised of one or more of four **primaries**: `options`, `tests`, `actions`, and `operators`. 

If you're not already, let's move into the `Unix-Tips` directory to practice some of these expressions.
```bash
cd Unix-Tips
```
## 1. `options`
These affect the overall operation. *Where* to search instead of *what* to search. Let's look at some of the most common `options`:
### `-maxdepth [N]`
Limits how many subdirectories deep your expression will search starting from the search root.
```bash
find . -maxdepth 2 -name "*.csv"
```
**Output**
```bash
./local-universe/galaxies.csv
```

### `-mindepth [N]`
Skips a number of directory levels before applying tests.
```bash
find . -mindepth 3 -name "*.csv"
```
**Output:**
```bash
./local-universe/milky-way/constellations.csv
./local-universe/milky-way/planets.csv
```

### `-depth`
Processes directory contents starting from the current directory without leaving behind empty directories or causing accidental deletion issues.

### `-xdev`/`-mount`
Prevents find from descending into directories that are on different filesystems than the starting point. This is especially useful when you're searching from a top-level directory like `/`, and you want to avoid scanning mounted drives, network shares, or other partitions (e.g., `/proc`, `/mnt`, `/media`, etc.).

## 2. `tests`
These return a true or false value depending on the file's attributes. Here are some examples for the most common `find` `tests`:
### `-name PATTERN`/`-iname PATTERN`
Matches files by name. `-iname` is case-insensitive. Wildcards like `*` and `?` are supported.
```bash
find . -name "pl*"
```
**Output:**
```bash
./local-universe/milky-way/planets.csv
```
### `-type`
Matches by file type:

* `f`: Regular file

* `d`: Directory

* `l`: Symbolic link
```bash
find . -type d
```
**Output:**
```bash
.
./local-universe
./local-universe/milky-way
```
### `-mtime N`/`-mmin N`
Matches files and folders by modification time. `-mtime` uses days, `-mmin` uses minutes. Let's say I've recently edited `galaxies.csv`, but none of the other files:
```bash
find . -mtime -1 # Find objects modified in the last day
```
**Output:**
```bash
.
./local-universe/galaxies.csv
```
```bash
find . -mmin +60 # Find objects modified more than an hour ago
```
**Output:**
```bash
.
./local-universe
./local-universe/milky-way
./local-universe/milky-way/constellations.csv
./local-universe/milky-way/planets.csv
```
### `-size N[b|c|k|M|G]`
Matches files by size.

* `b`: 512-byte blocks

* `c`: bytes

* `k`: Kb

* `M`: Mb

* `G`: Gb
```bash
find . -size +10M  # Find files larger than 10 Mb
find . -size -100k # Find files smaller than 100 Kb
find . -size 3b    # Find files that are exactly 3 blocks
```
### `-user NAME`/`-group NAME`
Find files owned by a specific user or group.
```bash
find /home -user admin # Find files owned by user named admin
```
>NOTE: `-nouser` and `-nogroup` can be used to find files with no associated user or group. Those commands are often used for deleted accounts. 
### `-empty`
Match empty files or directories
```bash
find . -type f -empty # Find empty files
find . -type d -empty # Find empty directories
```
### `-path`/`-wholename`
Match full relative path (from search root)
```bash
find . -path "./local-universe/milky-way/*.csv"
```
**Output:**
```bash
./local-universe/milky-way/constellations.csv
./local-universe/milky-way/planets.csv
```
### `-newer FILE`/`-anewer FILE`/`-cnewer FILE`
Match files based on their modified/access/change times respectively.
```bash
find . -newer constellations.csv # Find files modified more recently than constellations.csv
```
**Output:**
```bash
.
./local-universe
./local-universe/galaxies.csv
./local-universe/milky-way
```
### `-readable`/`-writable`/`-executable`
Checks the accessibilities for the current user.
```bash
find . -type f -writable
```
**Output:**
```bash
./local-universe/galaxies.csv
./local-universe/milky-way/constellations.csv
./local-universe/milky-way/planets.csv
```
## 3. `actions`
These have side effects, but otherwise also return a `True` or `False` value. Here are some of the most common `find` `actions`:
### `-print`
This is the default action which does not normally need to be specified. `-print` prints the patch of each file which matches the rest of the expression. It is useful to explicitly call `-print` when your expression includes actions or tests that don't automatically print their results, especially when chaining multiple conditions such as `-exec`, `-delete`, or `-ok`.
### `-fprint FILE`
This prints the result **into** a given `FILE` followed by a newline. This will also create the `FILE` if it does not already exist.
```bash
find . -name "*.csv" -fprint ./csv_list.txt 
```
This command creates a new file named "csv_list.txt" which contains a list of all the .csv files.
### `-delete`
Deletes each file which matches the expression: **Use with caution.** This command is often combined with `-type` to avoid deleting directories or special files by accident.
```bash
find ./Unix-Tips -name "*.txt" -delete 
```
### `-exec COMMAND {} \;`
Runs a command for each matching file. The shell substitutes `{}` with the full path to the file when running. `\;` then terminates the command.

You can also replace `\;` with `+` to have the command run **once** with multiple file arguments instead of once **per file**.
```bash
find . -name "*.txt" -exec gzip {} \;
```
This command will run `gzip` on all `.txt` files.
### `-prune`
Stops `find` from descending into matching directories. This action is common for skipping paths.
```bash
find local-universe -path "local-universe/milky-way" -prune -o -name "*.csv" 
```
**Output:**
```bash
local-universe/galaxies.csv
local-universe/milky-way
```
### `-quit`
Stops after the first match. Useful for fast existence checks.
```bash
find . -name "*.csv" -print -quit
```
**Output:**
```bash
./local-universe/galaxies.csv
```
### Less common `actions` that are still useful in scripts:
#### `-print0` and `-fprint0 FILE`
Prints filenames with a null character instead of a newline. This action is ideal for filenames which include spaces.
#### `-ls`
Prints `ls -dils` style info for each file.
#### `-ok COMMAND ;`
Like `-exec`, but prompts for confirmation before running each command.
#### `-execdir`, `-okdir`
Like `-exec`, but runs the command in the directory of the matching file instead of where `find` was run. Useful when the command depends on relative paths.
## 4. `operators`: 
`find` allows for you to combine multiple tests using **logical operators** such as **AND**, **OR**, and **NOT**. These combinations can then also be grouped with parentheses. `operators` can be very powerful for creating more precise search expressions:
### `-and`/`-a`/space
**All expressions** must be true. This is the default behavior of `find`: listing conditions with spaces will automatically assume `-and`, but it can be written explicitly.
```bash
find . -type f -and -name "*.csv"
```
This is equivalent to:
```bash
find . -type f -name "*.csv"
```
### `-or`/`-o`
Matches if **either** condition is true.
```bash
find . \( -name "galaxies.csv" -or -name "planets.csv" \)
```
**Output:**
```bash
./local-universe/galaxies.csv
./local-universe/milky-way/planets.csv
```
### `-not`/`!`
Inverts a test
```bash
find . -type f ! -name "planets.csv"
```
**Output:**
```bash
./local-universe/galaxies.csv
./local-universe/milky-way/constellations.csv
./csv_list.txt
```
### Grouping using `(` and `)`
Allows for the grouping of expressions. Must be escaped or quoted in most shells in order to avoid shell interpretation: `\(...\)` such as in the `-or` example.
### Operation Precedence
`find` lists the precedence order for operators from highest to lowest as:

1. `()`

2. `!`/`-not`

3. Then `-and`

4. Then `-or`

---

Now that we have a better handle on searching for specific files, [click here to continue on to the next section](06_searching_inside_files.md) to learn how to search for specific text inside of your files.