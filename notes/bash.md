---
layout: page
title: Bash Scripting Basics
description: >
    Notes I keep for myself while learning how to automate tasks using Bash scripts.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What is a shell script?

A shell script is a text file containing commands that a shell can execute.

Instead of manually entering commands into the terminal, I can write them in a file and execute that file.

For example, I could create a script that:

- Creates directories.
- Copies or moves files.
- Compiles a C program.
- Runs a program.
- Creates backups.
- Checks whether a file exists.
- Automates repetitive tasks.

Bash is one of the most commonly used shells on Linux.

A Bash script is essentially a small program made from shell commands, variables, conditions, loops, and functions.

## My first script

I'll create a file called `hello.sh`:

```bash
#!/usr/bin/env bash

echo "Hello, world!"
```

The first line is called the **shebang**.

```bash
#!/usr/bin/env bash
```

It tells the operating system which interpreter should execute the script.

In this case, it uses Bash.

The second line prints a message to the terminal.

### Executing the script

There are two common ways to run it.

**Method 1: Execute it through Bash**

```bash
bash hello.sh
```

**Method 2: Make it executable**

```bash
chmod +x hello.sh
```

Then execute it:

```bash
./hello.sh
```

The `chmod +x` command gives the file executable permission.

The `./` means that I'm executing a file in the current directory.

## Variables

I can store values in variables and use them later.

```bash
#!/usr/bin/env bash

name="Peasant"
age=30

echo "Hello, $name!"
echo "You are $age years old."
```

A few things to remember:

- No spaces around the `=` sign when assigning a value.
- Use `$` to retrieve a variable's value.
- Variable names are case-sensitive.

For example:

```bash
name="ChadGPT"

echo "$name"
```

I can also use braces to clearly separate a variable name from surrounding text:

```bash
name="Chad"

echo "${name}GPT"
```

Output:

```text
ChadGPT
```

### Command substitution

I can store the output of a command in a variable.

```bash
current_directory=$(pwd)

echo "I am currently in: $current_directory"
```

The command inside `$(...)` executes first, and its output is assigned to the variable.

This is called command substitution.

## Reading user input

I can use `read` to ask the user for information.

```bash
#!/usr/bin/env bash

echo "What is your name?"

read -r name

echo "Hello, $name!"
```

I can also combine the prompt and input:

```bash
read -r -p "What is your name? " name

echo "Hello, $name!"
```

The `-r` option prevents `read` from treating backslashes specially.

## Arguments

I can pass information to a script when executing it.

For example:

```bash
#!/usr/bin/env bash

echo "Hello, $1!"
```

Execute it:

```bash
bash hello.sh Peasant
```

Output:

```text
Hello, Peasant!
```

Bash provides special variables for accessing script arguments:

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `$#` | Number of arguments |
| `$@` | All arguments, individually |
| `$?` | Exit status of the previous command |
| `$$` | Process ID of the current shell |

For example:

```bash
#!/usr/bin/env bash

echo "Script: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Number of arguments: $#"
```

I can use `"$@"` to pass all arguments to another command while preserving arguments containing spaces.

## Conditions

Scripts can make decisions using `if`, `elif`, and `else`.

```bash
#!/usr/bin/env bash

read -r -p "Enter your age: " age

if (( age >= 18 )); then
    echo "You are an adult."
else
    echo "You are under 18."
fi
```

The `(( ... ))` syntax performs arithmetic evaluation in Bash.

### Checking files and directories

Bash provides useful tests for checking files and directories.

```bash
if [[ -f "main.c" ]]; then
    echo "The file exists."
else
    echo "The file does not exist."
fi
```

Some useful tests:

| Test | Meaning |
|---|---|
| `-e file` | File or path exists |
| `-f file` | Regular file exists |
| `-d directory` | Directory exists |
| `-r file` | File is readable |
| `-w file` | File is writable |
| `-x file` | File is executable |
| `-z "$value"` | String is empty |
| `-n "$value"` | String is not empty |

I can combine conditions:

```bash
if [[ -f "main.c" && -f "Makefile" ]]; then
    echo "Both files exist."
fi
```

Here, `&&` means AND.

I can also use `||` for OR.

## Loops

Loops allow me to repeat commands.

### The for loop

For example, I can print numbers from 1 to 5:

```bash
for number in 1 2 3 4 5; do
    echo "$number"
done
```

I can also use a numerical range:

```bash
for number in {1..5}; do
    echo "$number"
done
```

Or use C-style syntax:

```bash
for ((i = 0; i < 5; i++)); do
    echo "$i"
done
```

This should look familiar if I've been learning C.

### The while loop

A `while` loop continues as long as its condition is true.

```bash
number=1

while (( number <= 5 )); do
    echo "$number"
    ((number++))
done
```

This prints numbers from 1 to 5.

### Breaking a loop

I can use `break` to stop a loop immediately.

```bash
for number in {1..10}; do
    if (( number == 5 )); then
        break
    fi

    echo "$number"
done
```

I can use `continue` to skip the current iteration and move to the next one.

## Functions

Functions allow me to organize commands into reusable blocks.

```bash
#!/usr/bin/env bash

greet() {
    echo "Hello, $1!"
}

greet "Peasant"
greet "Master"
```

Output:

```text
Hello, Peasant!
Hello, Master!
```

Here, `$1` refers to the first argument passed to the function.

### Returning values

Bash functions can return an exit status using `return`.

```bash
is_file_present() {
    [[ -f "$1" ]]
}

if is_file_present "main.c"; then
    echo "Found main.c!"
else
    echo "main.c is missing."
fi
```

A function's exit status indicates success or failure.

Conventionally:

- `0` means success.
- A non-zero value means failure.

This is different from returning a regular value in C or JavaScript.

If I want a function to produce text that I can capture, I can use `echo` and command substitution:

```bash
get_greeting() {
    echo "Hello from Bash!"
}

message=$(get_greeting)

echo "$message"
```

## Exit status

Commands return an exit status when they finish.

Usually:

- `0` means success.
- Non-zero means something went wrong.

I can inspect the previous command's exit status with `$?`.

```bash
mkdir test_directory

echo $?
```

If the directory was created successfully, the status should be `0`.

I can use this to make scripts react to failures.

```bash
mkdir test_directory

if [[ $? -eq 0 ]]; then
    echo "Directory created successfully."
else
    echo "Something went wrong."
fi
```

A more direct approach is to put the command inside the condition:

```bash
if mkdir test_directory; then
    echo "Directory created successfully."
else
    echo "Could not create directory."
fi
```

This is generally preferable because I don't need to inspect `$?` separately.

## Redirection and pipes

Shell scripts can use the same redirection and piping techniques as the terminal.

### Redirecting output

```bash
echo "Hello!" > message.txt
```

This writes the output into a file, replacing its previous contents.

```bash
echo "Another message" >> message.txt
```

This appends the output instead.

### Redirecting errors

```bash
ls missing_directory 2> errors.txt
```

The `2>` redirects standard error into a file.

### Using pipes

```bash
ls -l | grep ".c"
```

The output of `ls -l` becomes the input of `grep`.

This allows me to combine small commands into more useful operations.

## Error handling

A script should ideally stop or react appropriately when something goes wrong.

One useful Bash option is:

```bash
set -e
```

This tells Bash to exit when certain commands fail.

Another option:

```bash
set -u
```

This treats the use of an unset variable as an error.

And:

```bash
set -o pipefail
```

This makes a pipeline report failure if a command within it fails, rather than only considering the final command.

A common combination is:

```bash
set -euo pipefail
```

However, these options have some subtleties, especially around conditions, pipelines, and commands expected to fail. I should understand their behavior before relying on them in important scripts.

## A practical example: compiling a C program

Here's a small script that compiles and runs a C program.

I'll call it `build.sh`:

```bash
#!/usr/bin/env bash

set -e

if [[ ! -f "main.c" ]]; then
    echo "Error: main.c was not found."
    exit 1
fi

echo "Compiling..."

gcc main.c -o main

echo "Compilation successful!"
echo "Running program..."

./main
```

Make it executable:

```bash
chmod +x build.sh
```

Run it:

```bash
./build.sh
```

The script checks whether `main.c` exists before attempting to compile it.

If the file is missing, `exit 1` terminates the script with a failure status.

Otherwise, it compiles the program and executes it.

## Useful Bash commands and concepts

| Command or syntax | Purpose |
|---|---|
| `echo` | Print text