# Shell Scripting
As the Unix shell is capable of using the contents of a file as input, it's possible to create a file with a list of commands which can then be executed by the shell. These files are called **Shell Scripts** or **Shell Programs**, and use the `.sh` file extension. Shell scripts work just like any other programming language, so having experience with other languages will make this section easier to understand and follow along with. Creating shell scripts can be useful for many applications, such as:

* Routing backups of files

* Adding new functionality to the shell

* Automating work and repetition, etc

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
```
## Shell Scripting Syntax
Like other programming languages, shell scripting allows for the definition of variables, user input, for and while loops, arithmetic expressions, logical operators, conditionals, and loops. Let's take a look at some examples of each of these.
### Variables
Variables can be defined with `variable_name=value`. Note that bash does not allow for spaces between the variable name, equals sign, and the value. 

To access the value in a later call, add `$` before the variable name.

Let's create a new file and start filling it with examples as we learn them:
```bash
nano examples.sh
```
```bash
#! /usr/bin/bash
greeting=Hello
subject=World
echo "$greeting, $subject!"
```
You can then save this file and make it executable using the `chmod +x` command from before to give it a try. From here on, feel free to edit `examples.sh` using the `nano` command as you see fit, such as by appending the new examples to the end of the file or by deleting the previous example. 
### Arithmetic
Bash supports the following operators for mathematic arithmetic:
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
It's also important to note that while writing equations in bash, it is mandatory to put a space between the value and the operator (`a + b` instead of `a+b`). Let's try out some examples by setting some numbers to variables and performing some calculations.
```bash
a=100
b=5
c=$((a / b))
d=$((c ** b))
e=$((d % a))
echo "c=$c, d=$d, e=$e"
```
**Output:**
```bash
c=20, d=3200000, e=0
```
By default, bash *only* returns integer values. In order to return floating point values it is necessary to use another tool such as `bc`. You can install `bc` with the command:
```bash
sudo apt install bc
```
Then you can add to `examples.sh`:
```bash
f=$(echo "scale=5; 22 / 7" | bc)
echo $f
```
(`scale` specifies the number of decimal places to return in the output.)
**Output:**
```
3.14285
```
### User Inputs
It is possible to take user input using the `read` command. To prompt the user with a custom message for user input, you can pair `read` with the `-p` flag. Let's say we want to create a very rudimentary calculator with user input. To our `examples.sh` file, we can add:
```bash
read -p "Enter the first number. " first_number
read -p "Enter the second number. " second_number
add_values=$((first_number + second_number))
echo "$first_number + $second_number = $add_values"
```
**Output:**
```bash
Enter the first number. 10
Enter the second number. 15
10 + 15 = 25
```
### Conditionals
Conditional statements in bash have two components: a conditional statement and one or more conditional operators.
#### Conditional Statements
There are 5 available conditional statements available in bash programming, using combinations of `if`, `then`, `elif`, or `else`, as well as switch statements using `case`. 

1. `if`
```bash
if [ expression ]
then
    statement
fi
```
This block will run the statement if the expression is evaluated as true.

2. `if-else`
```bash
if [ expression ]
then
    statement1
else
    statement2
fi
```
If the first expression is true, the first statement will run, otherwise the second statement will.

3. Else If ladder
```bash
if [ expression1 ]
then
    statement1
    statement2
elif [ expression2 ]
then
    statement3
    statement4
else
    statement5
fi
```
This allows for multiple conditions to be tested separately with different results depending on which of the expressions evaluates as true. If none evaluate as true, it would then run the `else` statement.

4. Nested `if`
```bash
if [ expression1 ]
then
    statement1
    statement2
else
    if [ expression2 ]
    then
        statement3
    fi
fi
```
This allows you to check an initial condition, and then check for a second condition based on the results of the first. It is possible to nest an arbitrary number of conditions.

5. Case statements
With a `case` statement, you are able to match patterns conditionally and return different results depending on the match. After finding a match, all associated statements are executed until reaching a `;;`. If no match is found, the exit status of the case is set as zero.
```bash
case word in
    "var1") statement1;;
    "var2") statement2;;
    "var3") statement3;;
esac
```
#### Conditional Operators
There are several possible conditional operators that can be used to test numerical arguments, files, and strings. To test numerical arguments, you can reference the following table:

| Operator | Meaning                  |
|---       |---                       |
|`-eq`     | Equal to                 |
|`-ne`     | Not equal to             |
|`-gt`     | Greater than             |
|`-ge`     | Greater than or equal to |
|`-lt`     | Less than                |
|`-le`     | Less than or equal to    |

[Click here for a complete list of bash operators and how to use them.](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)
### Logical Operators
Logical operators allow for you to combine or negate conditional expressions.

| Operator | Meaning                         | Example                           |
| ---      | ---                             | ---                               |
|`&&`      | Logical AND (both must be true) | `[ $a -gt 5 ] && [ $b -lt 10 ]`   |
|`\|\|`    | Logical OR (either can be true) | `[ $a -lt 5 ] \|\| [ $b -eq 10 ]` |
| `!`      | Logical NOT (negates condition) | `! [ -f file.txt ]`               |

You can also group conditions using double square brackets and logical keywords:
```bash
if [[ $a -gt 5 && $b -lt 10 ]]
then
    echo "Both conditions are true."
fi
```
Or:
```bash
if [[ $name != "admin" || $level -lt 3 ]]
then
    echo "Access denied."
fi
```
### Loops
Bash supports several types of loops that allow for you to repeat a block of code until a condition is met or for each item in a list.
#### `for` loop
The `for` loop iterates over a list of value:
```bash
for planet in Mercury Venus Earth Mars
do
    echo "Welcome to $planet"
done
```
**Output:**
```bash
Welcome to Mercury
Welcome to Venus
Welcome to Earth
Welcome to Mars
```
Bash also supports C-style syntax:
```bash
for (( i = 0; i < 5; i++ ))
do
    echo "i is $i"
done
```
**Output:**
```bash
i is 0
i is 1
i is 2
i is 3
i is 4
```
#### `while` loop
The `while` loop runs as long as a specified condition is true:
```bash
count=1
while [ $count -le 5 ]
do
    echo "Count is $count"
    ((count++))
done
```
**Output:**
```bash
Count is 1
Count is 2
Count is 3
Count is 4
Count is 5
```
#### `until` loop
The `until` loop runs until the condition becomes true - the opposite of a `while` loop:
```bash
count=1
until [ $count -gt 5 ]
do
    echo "Count is $count"
    ((count++))
done
```
**Output:**
```bash
Count is 1
Count is 2
Count is 3
Count is 4
Count is 5
```
#### `break` and `continue`
* `break` exits the loop entirely.

* `continue` skips the current iteration and moves to the next.
```bash
for i in {1..10}
do
    if [ $i -eq 4 ]
    then
        continue
    elif [ $i -eq 8 ]
    then
        break
    fi
    echo $i
done
```
**Output:**
```bash
1
2
3
5
6
7
```

---

Now that we have a basic understanding of the syntax of shell scripting, let's try putting it to use using our example files. [Click here to continue on to the next section](09_script_example.md) where we'll put this lesson to work with a shell script.