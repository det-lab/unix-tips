# Using `grep` to Search Inside Files
The `grep` command (a contraction of "global/regular expression/print") allows for searching through text files with commands to perform simple or recursive searches, while also allowing for multiple search terms, the ability to count matches, and even allowing users to pipe the output to other commands for further manipulation.

The basic syntax of `grep` follows:
```bash
grep [OPTIONS] PATTERNS [FILES]
```
`grep` will then search for the given `PATTERN` in each of the listed files, and then by default will simply print the related line. The available options for `grep` are separated into 4 different groups:
## 1: Pattern Selection and Interpretation
These options define how `grep` understands and applies your search pattern, allowing you to switch between simple string matching and the more powerful regular expressions, as well as allowing you to control case sensitivity and match boundaries. The most common options you'll use here are:

* `-E`/`--extended-regexp`
    * Allows your `PATTERN` to be interpreted as an extended regular expression, allowing for more complex syntax like `+`, `?`, and `|` without needing backslashes.

* `-F`/`--fixed-strings`
    * Treats your pattern as a **literal string** instead of a regular expression. This is faster for simple, exact matches.

* `-G`/`--basic-regexp`
    * Uses *basic* regular expressions, and is the default mode of `grep` unless overridden by another options. Special characters require escaping with this option.

* `-P`/`--perl-regexp`
    * Enables *Perl-compatible* regular expressions, allowing for even more advanced and expressive syntax.

* `-e PATTERN`/`--regexp=PATTERN`
    * Specifies the pattern directly; useful when mixing multiple patterns or reading from a script.

* `-f FILE`/`--file=FILE`
    * Loads the search patterns **from a file**, one pattern per line, comparing each line against another file. This is helpful for long lists of search terms.
```bash
grep -f patterns.txt data.txt
```

* `-i`/`--ignore-case`
    * Performs a case-insensitive search

* `--no-ignore-case`
    * Explicitly disable case-insensitive matching. This can be useful in scripts where flags may vary.

* `-w`/`--word-regexp`
    * Match whole words only, ignoring partial matches.
```bash
grep -w 'cat' file.txt # Matches "cat", not "concatenate"
```
* `-x`/`--line-regexp`
    * Matches **entire lines** exactly.

* `-z`/`--null-data`
    * Treat input as null-delimited. This option is useful for special binary or structured formats. Each "line" ends in a null byte (`\0`) instead of a newline.
## 2: Miscellaneous
These options don't directly affect how `grep` interprets or searches for patterns but help with controlling its **behaviour**, **output**, or **interaction** with the shell.

* `-s`/`--no-messages`
    * Suppresses error messages about unreadable files.

* `-v`/`--invert-match`
    * Selects lines that **do not** match the given pattern. Often used to exclude known entries or to filter out unwanted matches. 
## 3: Output Control
These options customize how `grep` displays its results. They are able to help you manage large outputs, script more effectively, and integrate with tools that expect specific formats.

### Basic Display Controls:

* `-m NUM`/`--max-count=NUM`
    * Stop reading a file after finding the first `NUM` matching lines.
```bash
grep -m 3 'error' -i log.txt # Case-insensitive; prints out first 3 error messages from log.txt.
```
* `-b`/`--byte-offset`
    * Prints the byte offset (position) of each matching line's start within the file.

* `-n`/`--line-number`
    * Show the line number for each match. This is especially helpful when working with large files.

* `--line-buffered`
    * Flushes output after every line instead of buffering. Useful for piping `grep` into tools that need live output.

### File Identification Options:

* `-H`/`--with-filename`
    * Always show the file name in the output. This is a default option when multiple files are searched.

* `-h`/`--no-filename`
    * Suppress the file name output, such as when searching for multiple files if you only care about the content, not which file the content is from.

### Match Content Refinement:

* `-o`/`--only-matching`
    * Prints only the part of each line that matches the search pattern.

* `-q`/`--quiet`/`--silent`
    * Suppresses all normal outputs. This is useful for testing the presence or absence of a pattern in scripts.
```bash
grep -q 'admin' users.txt && echo "User found." # Returns only the echo message if admin found in users.txt
```

### Binary and Encoding Options:

* `--binary-files=TYPE`
    * Choose how to handle binary files.
        * `binary`: Treat as binary (default).
        * `text`: Treat as if plain text.
        * `without-match`: Silently skip binary files.

* `a`/`--text` are equivalent to `--binary-files=text`

* `-I` is equivalent to `--binary-files=without-match`

### Directory and Recursion:

* `-d ACTION`/`--directories=ACTION`
    * Allows you to choose what to do with directories.
        * `read`: Read them as files.
        * `recurse`: Search inside of them recursively.
        * `skip`: Ignore them (this is the default unless `-r` is used).

* `-D ACTION`/`--devices=ACTION`
    * Similar to `-d`, but for device files, FIFOs, or sockets.

* `-r`/`--recursive`
    * Recursively searches subdirectories (does not follow symlinks).

* `-R`/`--dereference-recursive`
    * Like `-r`, but does follow symlinks.

### File Filtering:

* `--include=GLOB`
    * Restrict search to files matching a glob pattern.

* `--exclude=GLOB`
    * Skips files that match the pattern.

* `--exclude-from=FILE`
    * Read exclusion patterns (one per line) from a given file.

* `--exclude-dir=GLOB`
    * Skip directories that match the pattern.

### Summary Output:

* `-L`/`--files-without-match`  
    * Prints the names of files that contain no matches.

* `-l`/`--files-with-matches`  
    * Prints the names of files with at least one match.

* `-c`/`--count`
    * Prints a count of the number of matches per file instead of the actual matches.

### Formatting:

* `-T`/`--initial-tab`
    * Align fields properly if tab characters are present in the output.

* `-Z`/`--null`
    * Output a null character (`\0`) after file names. This can be useful when filenames contain spaces or newlines.

## 4: Context Control
When searching for a pattern in a file, sometimes the goal is to see not just the matching line, but also the surrounding context. `grep` provides a set of options to include lines before or after matches, which can be especially helpful in logs, source code, or debugging outputs.

### Showing Context Around Matches:
* `-B NUM`/`--before-context=NUM`
    * Show `NUM` lines **before** each match.

* `-A NUM`/`--after-context=NUM`
    * Show `NUM` lines **after** each match.

* `-C NUM`/`--context=NUM`/`-NUM`
    * Show `NUM` lines **before and after** each match.
```bash
grep -3 'error' diagnostics.txt
```
### Formatting Context Blocks:

* `--group-separator=SEP`
    * Insert `SEP` between blocks of matches when context is shown. Default is `--`.

* `--no-group-separator`
    * Suppresses the line between context-separated match blocks entirely.

These options are useful when post-processing `grep` output and want consistent formatting or no interruptions between context groups.

### Highlighting Matches:

* `--color[=WHEN]`/`--colour[=WHEN]`
    * Highlight matching text using ANSI escape sequences. `WHEN` can be:
        * `always`: Force color, even when not writing to a terminal.
        * `never`: Disable color.
        * `auto`: Color only if output goes to a terminal.

Most modern terminal setups default to `--color=auto`.

### Line Ending Handling:

* `-U`/`--binary`
    * Disable stripping of `\r` (carriage return) characters at the end of lines. This is useful when dealing with files from Windows systems (CRLF line endings). This option helps preserve exact formatting in mixed-platform environments.