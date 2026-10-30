---
layout: page
title: Linux Command Line Basics
description: >
    Notes I keep for myself while learning Linux commands, shell scripting, text processing, and system administration.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What I Know About the Linux Command Line

The Linux command line allows me to interact with the operating system by entering commands into a shell.

A shell is a program that interprets commands and executes them.

Some common shells include:

- `bash` — Bourne Again Shell.
- `sh` — A standard Unix shell interface, often provided by a compatible shell.
- `zsh` — Z Shell, commonly used on macOS.

I will primarily use Bash when working with my Raspberry Pi and Debian Linux.

A command generally has this structure:

```bash
command [options] [arguments]
```

For example:

```bash
ls -la /home
```

- `ls` is the command.
- `-la` represents options.
- `/home` is the argument.

I can usually find out how a command works using:

```bash
command --help
```

Or:

```bash
man command
```

For example:

```bash
man sed
```

I can press `q` to exit a manual page.

## 1. Understanding the Shell

Before learning individual commands, I want to understand some basic shell concepts.

### Current Directory

The shell maintains a current working directory.

I can see it using:

```bash
pwd
```

I can change directories using:

```bash
cd /home
```

Some useful shortcuts:

```bash
cd ~       # My home directory
cd ..      # Parent directory
cd -       # Previous directory
cd /       # Root directory
```

### Absolute and Relative Paths

An absolute path starts from the root directory:

```bash
/home/username/projects
```

A relative path starts from my current directory:

```bash
projects/cursus
```

I can use `.` to represent the current directory and `..` to represent its parent.

### Command Options

Many commands support short and long options.

```bash
ls -l
ls --all
```

Some commands accept combined short options:

```bash
ls -lah
```

This is equivalent to:

```bash
ls -l -a -h
```

However, option syntax depends on the individual command.

### Quoting

I use quotes to control how the shell interprets text.

```bash
echo "Hello world"
```

Double quotes allow variable expansion:

```bash
name="Peasant"
echo "Hello, $name"
```

Single quotes preserve the literal contents:

```bash
echo 'Hello, $name'
```

This prints `$name` rather than its value.

I can escape individual characters using a backslash:

```bash
echo "This is a \"quoted\" word"
```

Quoting is particularly important when working with filenames containing spaces.

```bash
cd "My Projects"
```

I should generally quote variables when using them as arguments:

```bash
file="my document.txt"

cat "$file"
```

## 2. Essential File and Directory Commands

These are commands I expect to use constantly.

| Command | Purpose |
|---|---|
| `pwd` | Print current directory |
| `ls` | List directory contents |
| `cd` | Change directory |
| `mkdir` | Create directory |
| `rmdir` | Remove an empty directory |
| `touch` | Create an empty file or update timestamps |
| `cp` | Copy files or directories |
| `mv` | Move or rename files |
| `rm` | Remove files or directories |
| `file` | Identify a file's type |
| `stat` | Display file information |
| `realpath` | Display an absolute path |
| `tree` | Display directories as a tree, if installed |

Examples:

```bash
mkdir projects
mkdir -p projects/cursus/src
```

The `-p` option creates missing parent directories as needed.

Copy a file:

```bash
cp main.c backup.c
```

Copy a directory recursively:

```bash
cp -r project project-backup
```

Rename a file:

```bash
mv old-name.txt new-name.txt
```

Remove a file:

```bash
rm old-name.txt
```

Remove a directory and its contents:

```bash
rm -r old-directory
```

**Warning:** `rm` normally deletes files without moving them to a recycle bin. I should inspect paths carefully before using it, particularly with `rm -r` and `rm -f`.

I should never run unfamiliar destructive commands just because they appear in a tutorial.

## 3. Reading and Inspecting Files

I have several commands available for displaying file contents.

| Command | Purpose |
|---|---|
| `cat` | Display file contents |
| `less` | Read a file interactively |
| `more` | Display text one screen at a time |
| `head` | Display the beginning of a file |
| `tail` | Display the end of a file |
| `nl` | Display numbered lines |
| `wc` | Count lines, words, and bytes |
| `file` | Identify file type |

### cat

```bash
cat file.txt
```

I can also concatenate multiple files:

```bash
cat first.txt second.txt
```

### less

```bash
less file.txt
```

Useful navigation:

- `Space`: Next page.
- `b`: Previous page.
- `/word`: Search forward.
- `n`: Next search result.
- `q`: Exit.

### head

```bash
head file.txt
```

Displays the first ten lines by default.

I can specify the number:

```bash
head -n 20 file.txt
```

### tail

```bash
tail file.txt
```

Displays the last ten lines by default.

I can follow a log file as new lines are added:

```bash
tail -f application.log
```

This is particularly useful when troubleshooting applications and servers.

### wc

```bash
wc file.txt
```

Displays line, word, and byte counts.

I can request individual counts:

```bash
wc -l file.txt  # Lines
wc -w file.txt  # Words
wc -c file.txt  # Bytes
wc -m file.txt  # Characters
```

## 4. Finding Files and Directories

I can use `find` to search through directories.

```bash
find . -name "main.c"
```

This searches from the current directory for files or directories named `main.c`.

Some useful examples:

```bash
find . -type f
find . -type d
find . -name "*.c"
find . -name "*.h"
find . -iname "*.txt"
```

- `-type f` searches for regular files.
- `-type d` searches for directories.
- `-name` matches names, case-sensitive.
- `-iname` matches names, case-insensitive.

I can search for files modified recently:

```bash
find . -mtime -1
```

This finds files modified less than 24 hours ago.

I can search for files larger than 10 MB:

```bash
find . -type f -size +10M
```

### locate

`locate` can search a prebuilt file index:

```bash
locate main.c
```

It can be faster than `find`, but its database may not include newly created files. It may need to be installed and updated separately.

## 5. Searching Text with grep

`grep` searches text for patterns.

This is one of the most useful commands I can learn.

```bash
grep "hello" file.txt
```

This prints lines containing `hello`.

Some useful options:

| Option | Purpose |
|---|---|
| `-i` | Ignore case |
| `-n` | Show line numbers |
| `-r` | Search directories recursively |
| `-l` | Show filenames containing matches |
| `-v` | Show lines that do not match |
| `-w` | Match whole words |
| `-c` | Count matching lines |
| `-E` | Use extended regular expressions |

Examples:

```bash
grep -i "error" application.log
grep -n "main" main.c
grep -r "TODO" .
grep -l "sqlite" *.c
grep -v "^#" config.txt
```

I can combine options:

```bash
grep -in "error" application.log
```

### Regular Expressions

`grep` can use regular expressions to search for patterns.

```bash
grep "^Hello" file.txt
```

Matches lines beginning with `Hello`.

```bash
grep "world$" file.txt
```

Matches lines ending with `world`.

```bash
grep "[0-9]" file.txt
```

Matches lines containing a digit.

```bash
grep -E "cat|dog" file.txt
```

Matches lines containing either `cat` or `dog`.

Regular expressions are useful for searching source code, configuration files, and logs.

## 6. Text Processing with printf

`printf` is one of the commands I particularly want to understand.

It formats and prints text.

Unlike `echo`, `printf` gives me precise control over formatting.

In Bash, `printf` is normally a shell builtin. There is also a standalone `printf` utility.

### Basic Usage

```bash
printf "Hello, world!\n"
```

The `\n` represents a newline.

I can print several values:

```bash
printf "Name: %s, Level: %d\n" "Alice" 10
```

Output:

```text
Name: Alice, Level: 10
```

### Format Specifiers

Some useful format specifiers:

| Specifier | Meaning |
|---|---|
| `%s` | String |
| `%d` | Signed decimal integer |
| `%i` | Integer |
| `%f` | Floating-point number |
| `%c` | Character |
| `%x` | Hexadecimal |
| `%o` | Octal |
| `%%` | Literal percent sign |

Examples:

```bash
printf "%s\n" "Hello"
printf "%d\n" 42
printf "%f\n" 3.14
printf "%x\n" 255
printf "%c\n" A
printf "Progress: 50%%\n"
```

### Formatting Numbers

I can control the number of decimal places:

```bash
printf "%.2f\n" 3.141592
```

Output:

```text
3.14
```

I can specify field width:

```bash
printf "%10s\n" "Hello"
```

This reserves ten character positions for the string.

I can left-align a value:

```bash
printf "%-10s %d\n" "Alice" 42
```

I can pad an integer with zeros:

```bash
printf "%05d\n" 42
```

Output:

```text
00042
```

### Escape Sequences

Some useful escape sequences:

| Sequence | Meaning |
|---|---|
| `\n` | Newline |
| `\t` | Tab |
| `\\` | Backslash |
| `\"` | Double quotation mark |
| `\r` | Carriage return |
| `\b` | Backspace |

Example:

```bash
printf "Name:\tAlice\nLevel:\t10\n"
```

### Printing Variables

```bash
username="Alice"
level=10

printf "Player: %s\nLevel: %d\n" "$username" "$level"
```

I should remember that Bash variables are generally treated as