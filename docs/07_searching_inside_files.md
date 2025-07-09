# Using `grep` to Search Inside Files
The `grep` command (a contraction of "global/regular expression/print") allows for searching through text files with commands to perform simple or recursive searches, while also allowing for multiple search terms, the ability to count matches, and even allowing users to pipe the output to other commands for further manipulation.

The basic syntax of `grep` follows:
```bash
grep [OPTIONS] [PATTERNS] [FILES]
```
`grep` will then search for the given `PATTERN` in each of the listed files, and by default will then simply print the related line. The available options for `grep` are separated into 4 different groups:
## 1: Pattern Selection and Interpretation
These options define how `grep` understands and applies your search pattern, allowing you to switch between simple string matching and the more powerful regular expressions, as well as allowing you to control case sensitivity and match boundaries. The most common options you'll use here are:

### `-i`/`--ignore-case`
Ignores case sensitivity.
```bash
grep -i 'spiral' ./local-universe/galaxies.csv
```
**Output:**
```bash
Andromeda, Andromeda, Spiral,
Milky Way (Center), Sagittarius, Spiral,
Triangulum, Triangulum, Spiral,
```
### `-w`/`--word-regexp`
Match with a whole word.
```bash
grep -w 'Al' ./local-universe/milky-way/constellations.csv
```
**Output:**
```bash
Cancer, Al Tarf,
```
Without the `-w`, `grep` would return:
```bash
Taurus, Aldebaran,
Cancer, Al Tarf,
Capricornus, Deneb Algedi,
Pisces, Alpherg
```
### `-E`/`--extended-regexp`
Allows your `PATTERN` to be interpreted as an **extended regular expression**. This allows for more complex syntax like `+`, `?`, and `|` without needing backslashes. For instance, to find entries that start with either "A" or "S", you could use an expression like:
```bash
grep -E '^(A|S)' ./local-universe/milky-way/constellations.csv
```
**Output:**
```bash
Aries, Hamal,
Scorpius, Antares,
Sagittarius, Kaus Australis,
Aquarius, Sadalsuud,
```
### `-e PATTERN`/`--regexp=PATTERN`
Search using multiple patterns.
```bash
grep -e 'Pisces' -e 'Taurus' ./local-universe/milky-way/constellations.csv
```
**Output:**
```bash
Taurus, Aldebaran,
Pisces, Alpherg
```
### `-f FILE`/`--file=FILE`
This allows you to use a file to provide the search patterns, loading one pattern per line.

Let's say we know a couple of the brightest stars in some constellations, but don't know the names of the constellations themselves. We can create a pattern file:
```bash
nano pattern.txt
```
And then fill it with the stars we know:
```
Hamal
Aldebaran
Al Tarf
Rasalhague
Deneb Algedi
Alpherg
```
Now you can use grep to find which lines contain those stars:
```bash
grep -f ./pattern.txt ./local-universe/milky-way/constellations.csv
```
**Output:**
```bash
Aries, Hamal,
Taurus, Aldebaran,
Cancer, Al Tarf,
Ophiuchus, Rasalhague,
Capricornus, Deneb Algedi,
Pisces, Alpherg
```
### `-F`/`--fixed-strings`
Treats your pattern as a **literal string** instead of a regular expression. A literal string is a search pattern that is matched **exactly as written** without treating any characters as special.
### `-G`/`--basic-regexp`
Uses *basic* regular expressions, and is the default mode of `grep` unless overridden by other options. Special characters require escaping with this option.
### `--no-ignore-case`
Explicitly disable case-insensitive matching. This can be useful when searching for lines that might contain the same spelling but with different capitalisation, such as `LEO` vs `Leo`.
### `-x`/`--line-regexp`
Matches **entire lines** exactly.
### `-z`/`--null-data`
Treat input as null-delimited. This option is useful for special binary or structured formats. Each "line" ends in a null byte (`\0`) instead of a newline.
## 2: Miscellaneous
These options don't directly affect how `grep` interprets or searches for patterns but help with controlling its **behaviour**, **output**, or **interaction** with the shell.
### `-s`/`--no-messages`
Suppresses error messages about unreadable files.
### `-v`/`--invert-match`
Selects lines that **do not** match the given pattern. Often used to exclude known entries or to filter out unwanted matches. For instance, let's say we want to invert the search from our `-f` example, `pattern.txt`.
```bash
grep -v -f pattern.txt ./local-universe/milky-way/constellations.csv
```
**Output:**
```bash
Constellation, Brightest Star,
Gemini, Pollux,
Leo, Regulus,
Virgo, Spica,
Libra, Zubeneschamali,
Scorpius, Antares,
Sagittarius, Kaus Australis,
Aquarius, Sadalsuud,
```
## 3: Output Control
These options customize how `grep` displays its results. They are able to help you manage large outputs, script more effectively, and integrate with tools that expect specific formats.
### `-m NUM`/`--max-count=NUM`
Stop reading a file after finding the first `NUM` matching lines.
```bash
grep -m 3 'ar' -i ./local-universe/galaxies.csv
```
**Output:**
```bash
Milky Way (Center), Sagittarius, Spiral,
Pisces Dwarf, Pisces, Irregular,
Leo A, Leo, Irregular
```
### `-b`/`--byte-offset`
Prints the byte offset (position) of each matching line's start within the file.
```bash
grep -b 'Spiral' ./local-universe/galaxies.csv
```
**Output:**
```bash
29:Andromeda, Andromeda, Spiral,
59:Milky Way (Center), Sagittarius, Spiral,
100:Triangulum, Triangulum, Spiral,
```
### `-n`/`--line-number`
Show the line number for each match. This is especially helpful when working with large files.
```bash
grep -n 'Spiral' ./local-universe/galaxies.csv
```
**Output:**
```bash
2:Andromeda, Andromeda, Spiral,
3:Milky Way (Center), Sagittarius, Spiral,
4:Triangulum, Triangulum, Spiral,
```
### `--line-buffered`
Flushes output after every line instead of buffering. Useful for piping `grep` into tools that need live output.
### `-H`/`--with-filename`
Always show the file name in the output. This is a default option when multiple files are searched.
```bash
grep -H 'Spiral' ./local-universe/galaxies.csv
```
**Output:**
```bash
./local-universe/galaxies.csv:Andromeda, Andromeda, Spiral,
./local-universe/galaxies.csv:Milky Way (Center), Sagittarius, Spiral,
./local-universe/galaxies.csv:Triangulum, Triangulum, Spiral,
```
### `-h`/`--no-filename`
Suppress the file name output, such as when searching for multiple files if you only care about the content, not which file the content is from.
### `-o`/`--only-matching`
Prints only the part of each line that matches the search pattern.
```bash
grep -o 'Spiral' ./local-universe/galaxies.csv
```
**Output:**
```bash
Spiral
Spiral
Spiral
```
### `-q`/`--quiet`/`--silent`
Suppresses all normal outputs. This is useful for testing the presence or absence of a pattern in scripts.
```bash
grep -q 'Spiral' ./local-universe/galaxies.csv && echo "Spiral found."
```
**Output:**
```bash
Spiral found.
```
### `--binary-files=TYPE`
Choose how to handle binary files.

* `binary`: Treat as binary (default).

* `text`: Treat as if plain text.

* `without-match`: Silently skip binary files.

>NOTE: `a`/`--text` are equivalent to `--binary-files=text`
>
> `-I` is equivalent to `--binary-files=without-match`

### `-d ACTION`/`--directories=ACTION`
Allows you to choose what to do with directories.

* `read`: Read them as files.

* `recurse`: Search inside of them recursively.

* `skip`: Ignore them (this is the default unless `-r` is used).

### `-D ACTION`/`--devices=ACTION`
Similar to `-d`, but for device files, FIFOs, or sockets.
### `-r`/`--recursive`
Recursively searches subdirectories (**does not** follow symlinks).
```bash
grep -r 'Earth'
```
**Output:**
```bash
./local-universe/milky-way/planets.csv:Earth, 6.37E6, 1, 5.97E24,
```
### `-R`/`--dereference-recursive`
Like `-r`, but **does** follow symlinks.
### `--include=GLOB`
Restrict search to files matching a glob pattern. For instance, if you only wish to search inside of `.txt` files, you can run something like:
```bash
grep -r --include="*.txt" 'Hamal'
```
**Output:**
```bash
pattern.txt:Hamal
```
### `--exclude=GLOB`
Skips files that match the pattern.
### `--exclude-from=FILE`
Read exclusion patterns (one per line) from a given file. Let's create a new file named `exclude-pattern.txt` and fill it with some files we want to avoid in a search:
```bash
nano exclude-pattern.txt
```
Which will then contain:
```txt
galaxies.csv
planets.csv
```
Then, if we wanted to use a recursive search to find only constellations with "Leo" in their names (skipping the Leo A galaxy), we could run:
```bash
grep -r --exclude-from=exclude-pattern.txt 'Leo'
```
**Output:**
```bash
local-universe/milky-way/constellations.csv:Leo, Regulus,
```
### `--exclude-dir=GLOB`
Skip directories that match the pattern. To skip the `milky-way/' subdirectory, you could run:
```bash
grep -r --exclude-dir="milky-way" 'Tri'
```
**Output:**
```bash
local-universe/galaxies.csv:Triangulum, Triangulum, Spiral,
```
### `-L`/`--files-without-match`  
Prints the names of files that contain no matches. To find files that don't contain the word "Spiral", you could use:
```bash
grep -rL 'Spiral'
```
**Output:**
```bash
exclude-pattern.txt
local-universe/milky-way/constellations.csv
local-universe/milky-way/planets.csv
csv_list.txt
pattern.txt
```
### `-l`/`--files-with-matches`  
Prints the names of files with at least one match.
```bash
grep -rl 'Leo'
```
**Output:**
```bash
local-universe/galaxies.csv
local-universe/milky-way/constellations.csv
```
### `-c`/`--count`
Prints a count of the number of matches per file instead returning the actual matches.
```bash
grep -rc 'Al'
```
**Output:**
```bash
exclude-pattern.txt:0
local-universe/galaxies.csv:0
local-universe/milky-way/constellations.csv:4
local-universe/milky-way/planets.csv:0
csv_list.txt:0
pattern.txt:4
```
### `-T`/`--initial-tab`
Align fields properly if tab characters are present in the output.
### `-Z`/`--null`
Output a null character (`\0`) after file names. This can be useful when filenames contain spaces or newlines.
## 4: Context Control
When searching for a pattern in a file, sometimes the goal is to see not just the matching line, but also the surrounding context. `grep` provides a set of options to include lines before or after matches, which can be especially helpful in logs, source code, or debugging outputs.
###  `-B NUM`/`--before-context=NUM`
Show `NUM` lines **before** each match.
```bash
grep -B 1 "Andromeda" ./local-universe/galaxies.csv
```
**Output:**
```bash
local-universe/galaxies.csv-Galaxy, Constellation, Type,
local-universe/galaxies.csv:Andromeda, Andromeda, Spiral,
```
### `-A NUM`/`--after-context=NUM`
Show `NUM` lines **after** each match.
```bash
grep -A 1 "Andromeda" ./local-universe/galaxies.csv
```
**Output:**
```bash
Andromeda, Andromeda, Spiral,
Milky Way (Center), Sagittarius, Spiral,
```
### `-C NUM`/`--context=NUM`/`-NUM`
Show `NUM` lines **before and after** each match.
```bash
grep -C 1 "Andromeda" ./local-universe/galaxies.csv
```
**Output:**
```bash
Galaxy, Constellation, Type,
Andromeda, Andromeda, Spiral,
Milky Way (Center), Sagittarius, Spiral,
```
### `--group-separator=SEP`
Insert `SEP` between blocks of matches when context is shown. Default is `--`.
### `--no-group-separator`
Suppresses the line between context-separated match blocks entirely.

These options are useful when post-processing `grep` output and want consistent formatting or no interruptions between context groups.
### `--color[=WHEN]`/`--colour[=WHEN]`
Highlight matching text using ANSI escape sequences. `WHEN` can be:

* `always`: Force color, even when not writing to a terminal.

* `never`: Disable color.

* `auto`: Color only if output goes to a terminal.

Most modern terminal setups default to `--color=auto`.

### `-U`/`--binary`
Disable stripping of `\r` (carriage return) characters at the end of lines. This is useful when dealing with files from Windows systems (CRLF line endings). This option helps preserve exact formatting in mixed-platform environments.

---

Now that we understand better how to search within files, let's learn how we can edit files from the command shell. [Click here to continue on to the next section.](08_editing_files.md)