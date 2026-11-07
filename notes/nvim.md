---
layout: page
title: Neovim Basics and Productivity
description: >
    Notes I keep for myself while learning to use Neovim more efficiently for programming and editing text.
hide_description: true
sitemap: false
tags: [guides]
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What is Neovim?

Neovim (or `nvim`) is a powerful text editor inspired by Vim.

Unlike conventional text editors, Neovim is designed around keyboard-driven editing.

Its main philosophy is that I should be able to perform most editing operations without constantly moving my hands between the keyboard and mouse.

Neovim is particularly useful for programming because it provides:

- Efficient navigation through files.
- Powerful text manipulation.
- Search and replacement.
- Macros for automating repetitive editing.
- Multiple buffers and windows.
- Plugin support.
- Language Server Protocol (LSP) integration.
- A built-in terminal.

The most important concept to understand is that **Neovim is a modal editor**.

This means that the keyboard behaves differently depending on the mode I'm currently using.

## Understanding the modes

Neovim has several modes, but these are the most important ones to learn.

### Normal mode

This is the default mode when I open Neovim.

I use it to navigate, delete, copy, paste, search, and manipulate text.

Press:

```text
Esc
```

to return to Normal mode from most other modes.

### Insert mode

This is where I type text normally.

I can enter Insert mode using several commands:

| Key | Action |
|---|---|
| `i` | Insert before the cursor |
| `a` | Insert after the cursor |
| `I` | Insert at the beginning of the line's text |
| `A` | Insert at the end of the line |
| `o` | Open a new line below |
| `O` | Open a new line above |
| `s` | Delete the character under the cursor and insert |
| `S` | Delete the current line's text and insert |

Press `Esc` to return to Normal mode.

### Visual mode

Visual mode allows me to select text before performing an operation.

There are three important variations:

| Key | Mode |
|---|---|
| `v` | Character-wise Visual mode |
| `V` | Line-wise Visual mode |
| `Ctrl + v` | Block-wise Visual mode |

I can move the cursor to extend or shrink the selection.

Once I have selected something, I can delete, copy, change, indent, or manipulate it.

### Command-line mode

I can enter commands by pressing:

```text
:
```

For example:

```vim
:w
```

saves the file.

```vim
:q
```

quits Neovim.

```vim
:wq
```

saves and quits.

```vim
:q!
```

quits without saving.

Some useful commands:

| Command | Purpose |
|---|---|
| `:w` | Save |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `:e filename` | Open a file |
| `:enew` | Create an empty buffer |
| `:saveas filename` | Save the current buffer under a new name |
| `:help` | Open help |
| `:checkhealth` | Check Neovim's configuration and environment |

## Basic navigation

Neovim uses several keys for moving around.

### Basic movement

| Key | Movement |
|---|---|
| `h` | Left |
| `j` | Down |
| `k` | Up |
| `l` | Right |

These keys are useful because I can navigate without moving my hands away from the home row.

However, I can also use the arrow keys if I prefer.

### Moving through words

| Key | Movement |
|---|---|
| `w` | Beginning of the next word |
| `e` | End of the current or next word |
| `b` | Beginning of the previous word |
| `W` | Beginning of the next whitespace-separated WORD |
| `E` | End of the current or next WORD |
| `B` | Beginning of the previous WORD |

There is a subtle difference between `w` and `W`.

For example:

```c
int main(void)
{
    printf("Hello, world!");
}
```

`w` treats punctuation as word boundaries, while `W` generally treats everything separated by whitespace as one WORD.

### Moving within a line

| Key | Movement |
|---|---|
| `0` | First column of the line |
| `^` | First non-whitespace character |
| `$` | Last character of the line |
| `g_` | Last non-whitespace character |

For example, if a line contains indentation:

```c
        int number = 42;
```

`0` takes me to the very beginning of the line.

`^` takes me to the `i` in `int`.

### Moving through the file

| Key | Movement |
|---|---|
| `gg` | Beginning of the file |
| `G` | End of the file |
| `:25` | Go to line 25 |
| `25G` | Go to line 25 |
| `Ctrl + d` | Scroll down approximately half a screen |
| `Ctrl + u` | Scroll up approximately half a screen |
| `zz` | Center the current line vertically |
| `zt` | Move current line to the top |
| `zb` | Move current line to the bottom |

### Moving between matching brackets

One particularly useful command for programming:

```text
%
```

It jumps between matching brackets, parentheses, and braces.

For example:

```c
int main(void)
{
    if (number > 10)
    {
        printf("Large number\n");
    }
}
```

If the cursor is on an opening `{`, pressing `%` takes me to its matching `}`.

This is extremely useful when working with nested structures.

## Operators: the foundation of Neovim editing

One of the most important concepts in Neovim is the combination of operators and motions.

An operator tells Neovim what to do.

A motion tells Neovim where to do it.

For example:

```text
dw
```

means:

- `d`: Delete.
- `w`: Move through the next word.

The result is deleting text from the cursor to the beginning of the next word.

Some important operators:

| Operator | Action |
|---|---|
| `d` | Delete |
| `c` | Change (delete and enter Insert mode) |
| `y` | Yank (copy) |
| `>` | Indent |
| `<` | Remove indentation |
| `gu` | Convert to lowercase |
| `gU` | Convert to uppercase |
| `g~` | Toggle case |

### Repeating an operator

Some operators can be doubled to apply to the entire line.

| Command | Action |
|---|---|
| `dd` | Delete the current line |
| `cc` | Change the current line |
| `yy` | Yank the current line |
| `>>` | Indent the current line |
| `<<` | Remove indentation from the current line |
| `gUU` | Convert the current line to uppercase |
| `guu` | Convert the current line to lowercase |

For example:

```text
dd
```

deletes the entire line.

```text
yy
```

copies the entire line.

```text
p
```

pastes after the cursor or below the current line.

```text
P
```

pastes before the cursor or above the current line.

## Text objects: editing inside parentheses, braces, and more

This is one of my favorite Neovim features.

Text objects allow me to operate on a meaningful piece of text without manually selecting it.

There are two important prefixes:

- `i`: Inside.
- `a`: Around.

For example:

```text
ci{
```

means:

- `c`: Change.
- `i`: Inside.
- `{`: Braces.

It deletes everything inside the nearest matching braces and enters Insert mode, while keeping the braces themselves.

### Useful text objects

| Text object | Meaning |
|---|---|
| `iw` | Inner word |
| `aw` | A word, including surrounding whitespace |
| `iW` | Inner WORD |
| `aW` | A WORD, including surrounding whitespace |
| `i"` | Inside double quotes |
| `a"` | Double-quoted string, including quotes |
| `i'` | Inside single quotes |
| `a'` | Single-quoted string, including quotes |
| `i(` | Inside parentheses |
| `a(` | Parentheses and their contents |
| `i{` | Inside curly braces |
| `a{` | Curly braces and their contents |
| `i[` | Inside square brackets |
| `a[` | Square brackets and their contents |
| `it` | Inside an HTML/XML tag |
| `at` | HTML/XML tag and its contents |
| `ip` | Inner paragraph |
| `ap` | A paragraph |

The curly brace objects also work with `}`:

```text
ci}
ca}
```

Likewise, parentheses can be written as either `(` or `)`.

### Practical examples

Imagine I have:

```c
printf("Hello, world!");
```

If my cursor is anywhere inside the string:

```text
ci"
```

I can replace the text inside the quotation marks.

The result might be:

```c
printf("Good morning!");
```

I can also use:

```text
di"
```

to delete the text inside the quotes without entering Insert mode.

Or:

```text
yi"
```

to copy the text inside the quotes.

Now consider:

```c
if (number > 10)
{
    printf("Large number\n");
}
```

If my cursor is inside the curly braces, I can use:

```text
ci{
```

to replace the contents of the block.

Or:

```text
da{
```

to delete the entire brace-delimited block, including the braces.

### More useful combinations

| Command | Result |
|---|---|
| `ciw` | Change the word under the cursor |
| `diw` | Delete the word under the cursor |
| `yiw` | Copy the word under the cursor |
| `ci(` | Change inside parentheses |
| `di{` | Delete inside curly braces |
| `ya[` | Copy square brackets and their contents |
| `ci"` | Change text inside double quotes |
| `da(` | Delete parentheses and their contents |
| `vap` | Select a paragraph including its surrounding whitespace |
| `vi{` | Visually select inside braces |

This is especially useful when editing C code, JavaScript functions, arrays, structures, and nested expressions.

## Counts: performing actions multiple times

I can place a number before many Neovim commands to repeat them.

For example:

```text
5j
```

moves down five lines.

```text
3w
```

moves forward three words.

```text
4dd
```

deletes four lines.

```text
3yy
```

copies three lines.

```text
5p
```

pastes five times.

I can also combine counts with operators and text objects.

For example:

```text
3dw
```

deletes three words.

```text
2ciw
```

changes the word under the cursor and repeats the operation according to the count. For a single word object, this is not usually the same as changing two separate words, so I should choose the motion carefully.

## Visual selection

Visual mode is useful when I want to select text manually.

### Character-wise selection

Press:

```text
v
```

Then move the cursor to select characters.

Useful commands:

| Key | Action |
|---|---|
| `v` | Start character selection |
| `d` | Delete selection |
| `y` | Copy selection |
| `c` | Change selection |
| `u` | Lowercase selection |
| `U` | Uppercase selection |
| `>` | Indent selection |
| `<` | Remove indentation |

### Line-wise selection

Press:

```text
V
```

This selects entire lines.

I can select several lines using `j` and `k`.

Then:

```text
d
```

deletes the selected lines.

Or:

```text
y
```

copies them.

### Block-wise selection

Press:

```text
Ctrl + v
```

This starts Visual Block mode.

It allows me to select a rectangular region across multiple lines.

This is particularly useful for editing columns of text.

For example, imagine this code:

```c
int health = 100;
int mana   = 50;
int level  = 1;
```

I can select a vertical block around the variable names or values and modify several lines together.

Useful operations:

- `I`: Insert text at the beginning of the selected block.
- `A`: Append text at the end of the selected block.
- `d`: Delete the selected block.
- `c`: Change the selected block.

After typing inserted text, I press `Esc` to apply the change to all selected lines.

## Multiple cursors

Many editors, such as VS Code, provide multiple cursors.

This allows me to place several cursors in different locations and edit all of them simultaneously.

Neovim does not provide this exact feature natively.

However, it offers several alternatives.

### Method 1: Visual Block mode

This is the native solution when I want to edit the same column across multiple lines.

For example:

```c
int health = 100;
int mana = 50;
int level = 1;
```

Suppose I want to add `player.` before every variable name.

I can:

1. Move to the beginning of the first line.
2. Press `Ctrl + v`.
3. Move down to select the corresponding column positions.
4. Press `I`.
5. Type `player.`.
6. Press `Esc`.

The result:

```c
int player.health = 100;
int player.mana = 50;
int player.level = 1;
```

This is not true multiple-cursor editing, but it is excellent for column-based modifications.

### Method 2: Search and replace

If I want to change every occurrence of a word, substitution is often better than multiple cursors.

For example:

```vim
:%s/health/hp/g
```

This replaces every occurrence of `health` with `hp` throughout the file.

I can also ask for confirmation before replacing:

```vim
:%s/health/hp/gc
```

The `c` means confirm each replacement.

### Method 3: Use a macro

If I need to repeat a complicated editing operation on several lines, I can record a macro.

I'll explain macros in a later section.

### Method 4: Install a multiple-cursor plugin

If I really want the VS Code-style experience, I can install a plugin.

One popular option is:

**vim-visual-multi**

Repository:

https://github.com/mg979/vim-visual-multi

It provides multiple cursors and multiple selections.

Its common default workflow uses:

- `Ctrl + n`: Start selecting the word under the cursor and add matching occurrences.
- `n`: Select the next occurrence.
- `N`: Select the previous occurrence.

The plugin provides additional commands for adding cursors and editing multiple selections.

Exact mappings can be customized, so I should consult its documentation after installation.

I don't need this plugin immediately. I recommend becoming comfortable with Visual Block mode, text objects, and macros first.

## Search

Searching is another essential Neovim feature.

Press:

```text
/
```

to search forward.

For example:

```text
/player
```

Searches for the word `player`.

Press:

```text
?
```

to search backward.

After searching:

| Key | Action |
|---|---|
| `n` | Next matching result |
| `N` | Previous matching result |
| `*` | Search forward for the word under the cursor |
| `#` | Search backward for the word under the cursor |

I can also enable highlighted search results:

```vim
:set hlsearch
```

And incremental search:

```vim
:set incsearch
```

To remove search highlighting:

```vim
:nohlsearch
```

### Search and replace

Neovim provides powerful substitution commands.

Replace the first occurrence on the current line:

```vim
:s/old/new/
```

Replace every occurrence on the current line:

```vim
:s/old/new/g
```

Replace every occurrence throughout the file:

```vim
:%s/old/new/g
```

Ask for confirmation:

```vim
:%s/old/new/gc
```

Replace only within lines 10 to 20:

```vim
:10,20s/old/new/g
```

The `%` means the entire file.

The `g` means all occurrences on each selected line.

The `c` asks for confirmation.

For example, I can rename a variable throughout my source file:

```vim
:%s/player_health/health/gc
```

## Repeating the last operation

This is a small feature that can save a surprising amount of time.

Press:

```text
.
```

The dot command repeats the last change.

For example:

1. Move to a word.
2. Press `ciw`.
3. Type a replacement.
4. Press `Esc`.
5. Move to another word.
6. Press `.`.

Neovim repeats the previous change.

This is particularly useful when editing repetitive code.

I can also repeat the last `f` or `t` search using:

| Key | Action |
|---|---|
| `;` | Repeat the last character search in the same direction |
| `,` | Repeat the last character search in the opposite direction |

For example, `f;` searches for the next semicolon on the current line.

## Macros: recording and repeating actions

Macros allow me to record a sequence of editing commands and replay them.

This is extremely powerful when I need to perform the same complicated operation repeatedly.

### Recording a macro

Press:

```text
qa
```

This begins recording a macro into register `a`.

Now perform some editing operations.

When finished, press:

```text
q
```

This stops recording.

### Playing a macro

Press:

```text
@a
```

This executes the macro stored in register `a`.

To repeat it:

```text
@@
```

This repeats the last executed macro.

I can also specify a count:

```text
10@a
```

This executes the macro ten times.

### Practical example

Imagine I have:

```text
health
mana
stamina
strength
```

I want to turn each line into a C declaration:

```c
int health = 0;
int mana = 0;
int stamina = 0;
int strength = 0;
```

I can record a macro that:

1. Inserts `int ` at the beginning.
2. Moves to the end of the line.
3. Appends ` = 0;`.
4. Returns to Normal mode.
5. Moves down one line.

Then I can replay that macro on the remaining lines.

Macros are particularly useful when the editing operation is too complicated for a simple search and replace.

## Registers: copying and pasting more effectively

Neovim uses registers to store copied or deleted text.

The unnamed register is used automatically by many delete, change, and yank operations.

I can also use named registers.

There are 26 named registers:

```text
a through z
```

### Copying into a named register

For example:

```text
"ayy
```

This copies the current line into register `a`.

### Pasting from a named register

```text
"ap
```

Pastes the contents of register `a` after the cursor.

### Using the system clipboard

If clipboard support is available, I can use:

```text
"+y
```

to copy selected text into the system clipboard.

And:

```text
"+p
```

to paste from the system clipboard.

The `+` register represents the system clipboard.

On macOS, this is especially useful when copying between Neovim and other applications.

If it doesn't work, I can check clipboard support:

```vim
:checkhealth
```

## Marks and jumping around the file

Marks allow me to save a location and return to it later.

For example:

```text
ma
```

sets a mark called `a` at the current cursor position.

Later:

```text
'a
```

jumps to the beginning of the line containing that mark.

And:

```text
`a
```

jumps to the exact cursor position.

This is useful when working on large source files.

### Jump history

Neovim remembers many of the places I've visited.

| Key | Action |
|---|---|
| `Ctrl + o` | Jump backward |
| `Ctrl + i` | Jump forward |

For example, if I jump to a function definition and then want to return to where I was, `Ctrl + o` is very useful.

## Working with multiple files

Neovim has three related concepts:

- Buffers.
- Windows.
- Tabs.

They are not the same thing.

### Buffers

A buffer represents a file or editing document loaded into Neovim.

I can open another file:

```vim
:e main.c
```

Open another:

```vim
:e player.c
```

List buffers:

```vim
:ls
```

Switch to another buffer:

```vim
:bnext
```

Go to the previous buffer:

```vim
:bprevious
```

Or use:

```vim
:buffer 2
```

to switch to buffer number 2, if it exists.

Close the current buffer:

```vim
:bdelete
```

### Windows and splits

A window is a view into a buffer.

I can split my screen horizontally:

```vim
:split
```

Or vertically:

```vim
:vsplit
```

I can also open another file in a split:

```vim
:vsplit player.c
```

Move between windows:

| Key | Action |
|---|---|
| `Ctrl + w`, then `h` | Move to left window |
| `Ctrl + w`, then `j` | Move to window below |
| `Ctrl + w`, then `k` | Move to window above |
| `Ctrl + w`, then `l` | Move to right window |
| `Ctrl + w`, then `w` | Cycle through windows |

Close the current window:

```vim
:q
```

Resize windows:

```vim
:resize 20
```

Sets the current window height to 20 lines.

```vim
:vertical resize 80
```

Sets its width to 80 columns.

### Tabs

A tab page contains one or more windows.

Create a new tab:

```vim
:tabnew
```

Move to the next tab:

```vim
:tabnext
```

Move to the previous tab:

```vim
:tabprevious
```

Close the current tab:

```vim
:tabclose
```

I should remember that tabs in Neovim are not necessarily individual files. They are collections of windows.

## Indentation and formatting

When programming, proper indentation makes code much easier to read.

Useful commands:

| Command | Action |
|---|---|
| `>>` | Indent current line |
| `<<` | Remove indentation |
| `==` | Auto-indent current line |
| `gg=G` | Auto-indent the entire file |
| `>G` | Indent from current line to end of file |
| `<G` | Remove indentation from current line to end of file |

I can also select multiple lines in Visual Line mode and press `>` or `<` to indent or unindent them.

### Adjusting indentation settings

For example:

```vim
:set expandtab
:set tabstop=4
:set shiftwidth=4
:set softtabstop=4
```

These settings mean:

- `expandtab`: Insert spaces instead of tab characters.
- `tabstop`: Display a tab character using four columns.
- `shiftwidth`: Use four spaces for indentation operations.
- `softtabstop`: Use four columns when pressing Tab or Backspace in Insert mode.

These are common settings for C and JavaScript projects, although project conventions can differ.

## Working with the built-in terminal

Neovim includes terminal functionality.

I can open a terminal inside Neovim:

```vim
:terminal
```

Or create a vertical split containing a terminal:

```vim
:vert terminal
```

This is useful when I want to compile and run a C program without leaving my editor.

For example:

```bash
gcc main.c -o main
./main
```

### Leaving terminal input mode

When I enter a terminal, my keyboard normally sends input directly to the shell.

To return to Normal mode:

```text
Ctrl + \ 
Ctrl + n
```

This is a two-key sequence:

1. Press `Ctrl + \`.
2. Release it.
3. Press `Ctrl + n`.

I can then navigate the terminal window like any other Neovim window.

To return to terminal input mode:

```text
i
```

## The Neovim file explorer

Neovim includes a basic file explorer through netrw.

I can open it with:

```vim
:Explore
```

Or open it in a vertical split:

```vim
:Vexplore
```

This allows me to navigate directories and open files without leaving Neovim.

There are also external file-tree plugins, but netrw is enough to get started.

## Working with LSP

LSP stands for Language Server Protocol.

It allows Neovim to communicate with language servers that understand programming languages.

For example, `clangd` provides C and C++ language intelligence.

Depending on my Neovim configuration and key mappings, LSP can provide:

- Diagnostics and error messages.
- Go to definition.
- Find references.
- Code completion.
- Rename symbols.
- Code actions.
- Hover documentation.

Some common LSP operations are:

| Operation | Typical action |
|---|---|
| Go to definition | Jump to where a function or variable is defined |
| Find references | Find where a symbol is used |
| Hover | Display documentation and type information |
| Rename | Rename a symbol throughout its references |
| Code action | Show available automated code changes |

The actual keyboard shortcuts depend on my configuration.

I should inspect my current configuration rather than assume every Neovim installation uses the same mappings.

For example, if I want to inspect the available LSP functions:

```vim
:help lsp
```

## Configuration

Neovim's configuration is written in Lua by default.

On macOS and Linux, the usual configuration directory is:

```text
~/.config/nvim/
```

The main configuration file is:

```text
~/.config/nvim/init.lua
```

I can open it directly:

```bash
nvim ~/.config/nvim/init.lua
```

### A simple configuration

Here is an example of some useful settings:

```lua
-- Show line numbers
vim.opt.number = true

-- Show relative line numbers
vim.opt.relativenumber = true

-- Use spaces instead of tabs
vim.opt.expandtab = true

-- Use four spaces for indentation
vim.opt.tabstop = 4
vim.opt.shiftwidth = 4
vim.opt.softtabstop = 4

-- Highlight search results
vim.opt.hlsearch = true

-- Show search matches while typing
vim.opt.incsearch = true

-- Enable mouse support
vim.opt.mouse = "a"

-- Enable persistent undo history
vim.opt.undofile = true

-- Use system clipboard when supported
vim.opt.clipboard = "unnamedplus"
```

I can change these settings and restart Neovim, or reload the configuration:

```vim
:source $MYVIMRC
```

I don't need to add every setting immediately. I should understand what each one does before changing my configuration.

## Useful shortcuts reference

Here is a compact reference of commands I expect to use frequently.

### Editing

| Command | Action |
|---|---|
| `i` | Insert before cursor |
| `a` | Insert after cursor |
| `o` | New line below |
| `O` | New line above |
| `x` | Delete character |
| `dd` | Delete line |
| `yy` | Copy line |
| `p` | Paste after |
| `P` | Paste before |
| `u` | Undo |
| `Ctrl + r` | Redo |
| `.` | Repeat last change |
| `ciw` | Change word |
| `ci"` | Change inside quotes |
| `ci{` | Change inside braces |
| `di(` | Delete inside parentheses |
| `:%s/old/new/g` | Replace throughout file |

### Navigation

| Command | Action |
|---|---|
| `h j k l` | Move left, down, up, right |
| `w` | Next word |
| `b` | Previous word |
| `e` | End of word |
| `0` | Beginning of line |
| `^` | First non-whitespace character |
| `$` | End of line |
| `gg` | Beginning of file |
| `G` | End of file |
| `%` | Matching bracket |
| `*` | Search word under cursor |
| `n` | Next search result |
| `Ctrl + o` | Previous jump location |

### Visual mode

| Command | Action |
|---|---|
| `v` | Character selection |
| `V` | Line selection |
| `Ctrl + v` | Block selection |
| `d` | Delete selection |
| `y` | Copy selection |
| `c` | Change selection |
| `>` | Indent selection |
| `<` | Unindent selection |

### Files and windows

| Command | Action |
|---|---|
| `:w` | Save |
| `:q` | Quit |
| `:e file` | Open file |
| `:Explore` | Open file explorer |
| `:split` | Horizontal split |
| `:vsplit` | Vertical split |
| `:bnext` | Next buffer |
| `:bprevious` | Previous buffer |
| `:terminal` | Open terminal |

## Learning resources

Neovim has excellent built-in documentation.

I can access it from inside the editor:

```vim
:help
```

Some particularly useful help topics:

```vim
:help motion.txt
:help operator
:help text-objects
:help visual-mode
:help registers
:help macros
:help windows
:help options
```

Neovim also includes an interactive tutorial:

```vim
:Tutor
```

This is an excellent way to practice the fundamentals.

I can also visit:

- https://neovim.io/doc/
- https://neovim.io/

## My current understanding

Neovim is a modal editor. Instead of relying on mouse-driven editing, I can combine operators, motions, and text objects to manipulate text efficiently.

The most important concepts I want to become comfortable with are:

1. Normal, Insert, Visual, and Command-line modes.
2. Navigating with motions.
3. Combining operators with motions.
4. Using text objects such as `ci{`, `di(`, and `yi"`.
5. Visual Block mode for editing multiple lines.
6. Search and replacement.
7. Macros for repetitive editing.
8. Registers for copying and pasting.
9. Buffers, windows, and tabs.

I don't need to memorize everything. I should learn a few commands, use them regularly, and gradually add more to my editing vocabulary.

The goal is not to type complicated commands just for the sake of it. The goal is to edit code with less friction and spend more time thinking about the program itself.
