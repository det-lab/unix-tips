# Shell Scripting
As the Unix shell is capable of using the contents of a file as input, it's possible to create a file with a list of commands which can then be executed by the shell. These files are called **Shell Scripts** or **Shell Programs**, and use the `.sh` file extension. Shell scripts work just like any other programming language, so having experience with other languages will make this section easier to understand. Creating shell scripts can be useful for many applications, such as:

* Routing backups of files

* Adding new functionality to the shell

* Automating work and repitition, etc

Fortunately, the commands and syntax for shell scripting are the same as those entered directly in the command line, so this entire lesson so far can be applied directly in shell scripts.

## Creating Your First Shell Script
To begin, create a file ending in `.sh`, such as `hello.sh`:
```bash
touch hello.sh
```
### Bash Bang/Shebang
In order to tell the system to use the shell to interpret the commands, you'll have to start the file with a **bash bang**/**shebang** (a combination of `bash #` and `bang !`, followed by the bash shell path). 

The path can vary, so before we start working on our `.sh` file, you'll want to confirm the path to your bash shell with:
```bash
which bash
```
**Output:**
```bash
/usr/bin/bash
```
Now you can open your script with:
```bash
nano hello.sh
```
And add your shebang line along with some commands:
```bash
#! /usr/bin/bash

echo "Hello, world!"
date
```
You can then save and close the file.
### Execution rights:
In order to be able to actually treat the file as an executable, you'll have to modify the file's permissions. Users are able to have read, write, or execute (`rwx`) rights for files. To add execution rights for this file, you can run:
```bash
chmod +x hello.sh
```
You should now be able to execute the file by running:
```bash
./hello.sh
```
**Output:**
```bash
Hello, world!
Wed Jul  2 18:19:48 MDT 2025
```
## Shell Scripting Syntax
Like other programming languages, shell scripting allows for the definition of variables, user input, for and while loops, arithmetic expressions, logical operators, conditionals, and the reading of files. Let's take a look at some examples of each of these.
### Variables
Variables can be defined with `variable_name=value`. Note that bash does not allow for spaces between the variable name, equals sign, and the value. Then, to access the value of the variable in a later call, add `$` before the variable.

Let's create a new file and start filling it with examples as we learn them:
```bash
nano examples.sh
```
```bash
#! /usr/bin/bash
greeting=Hello
user=World
echo "$greeting, $user!"
```
You can then save this file and make it executable using the `chmod +x` command from before to give it a try.
### Arithmetic
Bash supports the following operators for mathmatic arithmetic:
| Operator | Use            |
|---       |---             |
| `**`     | Exponentiation |
| `*`      | Multiplication |
| `/`      | Division       |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `%`      | Modulus        |
Storing expressions in variables uses the syntax of:
```bash
var=$((expression))
```
Let's try out some examples by setting some numbers to variables and performing some calculations. If you closed `examples.sh`, simply open it again with the `nano` command.
```bash
a=100
b=5
c=$((a / b))
echo $c
```
### User Inputs
It is possible to take user input using the `read` command. To prompt the user with a custom message for user input, you can pair `read` with the `-p` flag. 

### Conditionals

### Logical Operators

### Reading Files