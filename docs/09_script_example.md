# Scripting Example
We've now learned how to use the Unix shell to navigate our folder system, to create and delete files, create and delete folders, copy, move, and rename files and folders, to search for and inside of files, how to edit the contents of files, and finally how to create and run shell scripts to automate any and all of these processes. Let's try now to put together some of these abilities of the Unix shell and create a script to manipulate the contents of our `Unix-Tips` folder. If you've been following along with the examples provided in this lesson, your folder tree should look similar to:
```bash
├── csv_list.txt
├── examples.sh
├── exclude-pattern.txt
├── hello.sh
├── kilogram-replacement.txt
├── local-universe
│   ├── galaxies.csv
│   └── milky-way
│       ├── constellations.csv
│       ├── planets.csv
│       └── planets.csv.bak
└── pattern.txt

2 directories, 10 files
```
In order to demonstrate some ways we can use shell scripts to edit files and folders, let's say that we want to create a script to accomplish the following goals with the contents of this folder:

1. Delete all of the `.txt` files

2. Restore the original version of `planets.csv`

3. Move `planets.csv` into a new `solar-system` folder

4. Change uppercase characters to lowercase for all `.csv` files.

Let's name our new script `reformat-folder.sh`:
```bash
nano reformat-folder.sh
```
>NOTE: When first creating a code it's useful to print statements (using `echo` for shell) for every step. This is makes it easier to find at what step an error occurs if you run into one.
```bash
#! /usr/bin/bash

# Set existing folders as variables for easier use
local_dir="./local-universe/"
galaxy_dir=$local_dir"milky-way/"

# Delete all .txt files
echo "Deleting .txt files..."
find . -type f -name "*.txt" -delete

# Forcefully overwrite planets.csv with the backup
echo "Restoring file from backup..."
src=$galaxy_dir"planets.csv.bak"
dest=$galaxy_dir"planets.csv"
cp -f "$src" "$dest"
# Note: quoting variable names for paths is useful if the path contains spaces

# Create a new folder for planets.csv
echo "Creating new folder..."
solar_dir=$galaxy_dir"solar-system/"
mkdir -p "$solar_dir"

# Move planets.csv into the new folder
echo "Moving planets.csv..."
src2=$galaxy_dir"planets.csv"
dest2=$solar_dir"planets.csv"
mv "$src2" "$dest2"

# Delete .bak file
echo "Deleting planets.csv.bak..."
bak_path=$galaxy_dir"planets.csv.bak"
rm "$bak_path"

# Loop through all .csv files and make everything lowercase
echo "Creating lowercase versions of all .csv files..."
find local-universe/ -type f -name "*.csv" | while read -r csv_file; do
    # Extract base name and directory
    dir_name=$(dirname "$csv_file")
    base_name=$(basename "$csv_file" .csv)

    # Define lowercase output filename
    out_file="${dir_name}/${base_name}-lowercase.csv"

    # Check if file is not already lowercase version
    if [[ ! "$csv_file" =~ lowercase\.csv$ ]]; then
        echo " Changing case: $csv_file -> $out_file"
        tr '[:upper:]' '[:lower:]' < "$csv_file" > "$out_file"
    fi
done

# Optional: Delete all non-lowercase versions of the files
echo "Deleting original .csv files..."
for file in $(find . -type f -name "*.csv"); do
    if [[ "$file" != *-lowercase.csv ]]; then
        echo "Deleting $file"
        rm "$file"
    fi
done

echo "Script complete!"
```
**Output:**
```bash
Deleting .txt files...
Restoring file from backup...
Creating new folder...
Moving planets.csv...
Deleting planets.csv.bak...
Creating lowercase versions of all .csv files...
 Changing case: local-universe/galaxies.csv -> local-universe/galaxies-lowercase.csv
 Changing case: local-universe/milky-way/constellations.csv -> local-universe/milky-way/constellations-lowercase.csv
 Changing case: local-universe/milky-way/solar-system/planets.csv -> local-universe/milky-way/solar-system/planets-lowercase.csv
Deleting original .csv files...
Deleting ./local-universe/galaxies.csv
Deleting ./local-universe/milky-way/constellations.csv
Deleting ./local-universe/milky-way/solar-system/planets.csv
Script complete!
```
You can then double check the script's work by running `tree`:

**Output**
```bash
├── examples.sh
├── hello.sh
├── local-universe
│   ├── galaxies-lowercase.csv
│   └── milky-way
│       ├── constellations-lowercase.csv
│       └── solar-system
│           └── planets-lowercase.csv
└── reformat-folder.sh
```
# Wrapping up
This short and simple script demonstrates some of the utility of the Unix shell when paired with scripting. A few lines of code allowed us to clean up files, restore backups, reorganize folders, and transform file contents, all automatically. While the script we wrote here is specific to the structure of this lesson, the techniques used here are widely applicable to any kind of filesystem or data maintenance task.

With careful use of conditionals, loops, and external tools, we can build reliable, repeatable processes that can save time and reduce human error. This is the core strength of Unix: simple commands that combine into powerful tools. As you continue learning, try experimenting with your own scripts to automate parts of your workflow and explore what the shell has to offer.