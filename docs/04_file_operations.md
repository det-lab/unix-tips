# Working with Files
File operations are fundamental to using the Unix shell effectively. Whether you're managing project files, organizing backups, or scripting automation tasks, mastering a few essential commands can greatly streamline your workflow.
## Creating files with `touch`
The `touch` command follows the basic format of:
```bash
touch [OPTION] FILE
```
The command updates the access and modification times of the selected file to the current time. It can also be used to create a new file if the `FILE` argument does not exist.

While there aren't options to influence the creation of a new file with `touch` (aside from **not** creating one using `-c` or `--no-create`), there are options to modify the access and modification times of an existing file. Some of the more common options for `touch` include:

### `-a`
Change only the access time.

### `-d [STRING]`/`--date=STRING`
Parse `STRING` and use it instead of the current time. 

`STRING` must be in the format `[[CC]YY]MMDD`. If `YY` is specified but `CC` (century) is not, a value for `YY` between 69 and 99 results in a `CC` value of 19. Otherwise, `CC` defaults to a value of 20. So, if you wanted to change the last modified date of a file to be January 1st, 2018, you would use the command:
```bash
touch -d 20180101 example.txt
```

### `-h`/`--no-dereference`
This command affects symbolic links (symlinks) instead of referenced files.

### `-m`
Changes the modification time to the current time.

### `-t [STAMP]`
Parse a `STAMP` of the format `[[CC]YY]MMDDhhmm[.SS]` to be the new modification time.

The same rules as in `-d` apply for the `[[CC]YY]MMDD` part of the argument. The `hh` argument uses a 24 hour clock (from 00 to 23). For example, if you wished to change the last modified date of a file to January 1st, 2018, at 5:28:30pm, you could use the command:
```bash
touch -d 201801011728.30 example.txt
```
After making a text file, it is common to edit the file using a **text editor**. For more information on the basics of text editors, you can follow [this link to a short series of lessons](https://det-lab.github.io/terminal-text-editors/) on three of the most popular text editors, `nano`, `Vim`, and `Emacs`.
## Making folders with `mkdir`
The `mkdir` command will create a new folder if one does not already exist. It follows the basic format of:
```bash
mkdir [OPTION] DIRECTORY
```
Some of the most common options used with `mkdir` include:
### `-p`/`--parents` 
Creates parent directories if necessary. For instance, running the following command without first creating `testdir` or `subdir` will create all three directories:
```bash
mkdir -p testdir/subdir/exampledir
```
### `-v`
Print a message for each created directory. This can be useful for automated scripts to show everything that gets created, potentially helping to find errors in directory creation.
## Copying files with `cp`
`cp` copies files or directories. The basic usage looks like:
```bash
cp [OPTION] SOURCE DEST
```
Where `SOURCE` and `DEST` would be the old and new filenames. For instance, if you're just copying a file to the same directory and changing its name, that would look like:
```bash
cp oldfile.txt backup.txt
```
You can also copy the original file into a new folder by setting `DEST` as a directory, such as in:
```bash
mkdir testdir
cp oldfile.txt testdir/
```
It is also possible to change the name of the copy in the same way:
```bash
cp oldfile.txt tesdir/backup.txt
```
Both `SOURCE` and `DEST` can be set as full paths for more precision if needed.

Some of the most common options used with `cp` include:

### `-f`/`--force`
If an existing destination file cannot be opened, remove it and try again.

### `-n`/`--no-clobber`
Prevents you from overwriting an existing file.

### `-i`/`--interactive`
Prompts you for confirmation before overwriting an existing file.

>NOTE:  Note that `-n` and `-i` will override each other. Only the second one will be used.

### `-R`/`-r`/`--recursive`
Option required to copy a directory. 

### `-s`/`--symbolic-link`
Makes a symbolic link instead of copying the file. A symbolic link is similar to a shortcut, differing from copying in that changes to the original will effect the symbolic link version of the file as well, while copying creates an independant version of the file.

## Move or rename files with `mv`
`mv` moves files if the target is a directory or renames them if the target includes a new name. This command differs from copying in that only one version of the file or directory is preserved. However the command is otherwise very similar, following the basic usage of:
```bash
mv [OPTION] SOURCE DEST
```
For instance, if you want to move a file to a new directory, you can use:
```bash
mv oldfile.txt testdir/
```
If you wish to rename it in the process:
```bash
mv oldfile.txt testdir/newname.txt
```
If you only wish to rename the file while keeping it in the same directory:
```bash
mv oldfile.txt newname.txt
```
`mv` shares many of its commands with `cp`, including:

* `-f`, `--force`

* `-i`, `--interactive`, and

* `-n`, `--no-clobber`

However, `-R`, `-r`, `--recursive`, `-s`, and `--symbolic-link` are not available for the `mv` command. 
## Delete files with `rm`
`rm` deletes/unlinks files or folders, so use this option with caution. Its basic usage follows the format of:
```bash
rm [OPTION] FILE
```
Like with `cp` and `mv`, the `FILE` argument can be a path to a specific file outside of the directory that your shell is inside of.

Some of the more common options for `rm` include:

### `-f`/`--force`
Ignores nonexistent files and arguments.

### `-i`
Prompts the user for confirmation before every removal.

### `-I`
Prompts the user for confirmation if more than three files would be deleted, or when removing recursively. This is useful for automated scripts.

###  `-r`/`-R`/`--recursive`
Remove directories and their contents.

### `-d`/`--dir`
Removes empty directories.
## Destroy files with `shred`
`shred` is similar to `rm` in that it makes files unusable, but `shred` repeatedly overwrites the content, making it **unrecoverable**. Think of `rm` as like throwing a file in the garbage, while `shred` is more like dumping ink over it. It is still possible to recover files that have been removed with `rm` if you know what you're doing, but it is incredibly difficult to recover files that you `shred`, even using expensive hardware. Its basic usage follows the format of:
```bash
shred [OPTION] FILE
```
When you `shred` a file, there will still be a file with the given name and extension, but its contents will be unrecognizable. To demonstrate, try making an example file with a text editor:
```bash
nano example.txt
```
Then, write something to the file, save and quit (`CTRL+X` $\to$ `Y` $\to$ `Enter`), and `shred` the file.
```bash
shred example.txt
```
You can then re-run the command `nano example.txt` to open the file in your shell to see that your message has been transformed into a jumble of meaningless characters.

Some of the more common options for `shred` include:

### `-f`/`--force`
Changes permissions to allow writing to the file if it would otherwise not be possible to `shred` the file.

### `-n N`/`--iterations=N`
By default, `shred` will overwrite the file 3 times. This option allows you to specify how many times, `N`, to overwrite the file, making it potentially easier or more difficult to recover the data.
```bash
shred -n 10
```

### `-s N`/`--size=N`
Allows you to specify a number, `N`, of bytes to overwrite.
```bash
shred -s 8
```
This command will only overwrite the first 8 bytes of a file.

### `-u`
Remove file after overwriting it.

### `-z` or `--zero`
Add a final overwrite filled with zeros to hide the shredding.

---

Now that you have a handle on creating, destroying, moving, and entering files or directories, [click here to continue to the next section](05_creating_example_files.md) where we can start creating some example files to practice searching with.