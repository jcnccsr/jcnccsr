---
layout: page
title: C Basics
description: >
  This chapter covers the basics of C programming language.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What Is C?

C is a compiled, general-purpose programming language. A C program is written in source code, compiled into machine code, and then executed.

A basic C program:

```c
#include <stdio.h>

int main(void)
{
    printf("Hello, world!\n");
    return 0;
}
```

- `#include <stdio.h>` includes declarations for standard input/output functions such as `printf()`.
- `int main(void)` defines the program's entry point.
- `{}` enclose a block of code.
- `printf()` prints formatted output.
- `\n` represents a newline.
- `return 0;` indicates successful program termination.
- `;` terminates a statement.

### Compiling and Running

Using GCC:

```bash
gcc main.c -o main
./main
```

To enable useful compiler warnings:

```bash
gcc -Wall -Wextra -Wpedantic main.c -o main
```

## Variables and Data Types

A variable stores a value of a particular type.

```c
int age = 25;
float temperature = 36.5f;
double pi = 3.1415926535;
char letter = 'A';
```

Common types:

| Type | Purpose |
|---|---|
| `char` | Character or small integer |
| `int` | Integer |
| `float` | Floating-point number |
| `double` | Floating-point number with greater precision than `float` |
| `void` | No value or unspecified type, depending on context |

The exact sizes and ranges of many types depend on the implementation.

Constants can be declared with `const`:

```c
const int DAYS_IN_WEEK = 7;
```

## Operators

### Arithmetic

```c
int a = 10;
int b = 3;

a + b;  // Addition
a - b;  // Subtraction
a * b;  // Multiplication
a / b;  // Division
a % b;  // Remainder
```

Integer division discards the fractional part: `10 / 3` produces `3`.

### Comparison

```c
a == b;  // Equal
a != b;  // Not equal
a < b;   // Less than
a > b;   // Greater than
a <= b;  // Less than or equal
a >= b;  // Greater than or equal
```

Comparisons produce `0` for false and `1` for true.

### Logical

```c
a && b;  // Logical AND
a || b;  // Logical OR
!a;      // Logical NOT
```

In C, `0` represents false, and nonzero values represent true.

## Conditional Statements

Use `if`, `else if`, and `else` to choose which code executes.

```c
int age = 20;

if (age >= 18)
{
    printf("Adult\n");
}
else
{
    printf("Minor\n");
}
```

The `switch` statement selects a branch based on an integer or compatible expression.

```c
switch (choice)
{
    case 1:
        printf("One\n");
        break;
    case 2:
        printf("Two\n");
        break;
    default:
        printf("Other\n");
}
```

`break` prevents execution from falling through into the next case.

## Loops

Loops repeat code.

### `for`

Useful when the iteration structure is known.

```c
for (int i = 0; i < 5; i++)
{
    printf("%d\n", i);
}
```

### `while`

Repeats while the condition is true.

```c
int i = 0;

while (i < 5)
{
    printf("%d\n", i);
    i++;
}
```

### `do-while`

Executes the body at least once.

```c
int i = 0;

do
{
    printf("%d\n", i);
    i++;
}
while (i < 5);
```

`break` exits a loop. `continue` skips to the next iteration.

## Functions

Functions organize code into reusable units.

```c
int add(int a, int b)
{
    return a + b;
}
```

Calling the function:

```c
int result = add(3, 4);
```

A function declaration, also called a prototype, tells the compiler about a function before its definition.

```c
int add(int a, int b);

int main(void)
{
    printf("%d\n", add(3, 4));
    return 0;
}

int add(int a, int b)
{
    return a + b;
}
```

C passes function arguments by value. A function receives copies of the argument values. To modify a variable belonging to the caller, a function can receive a pointer to it.

## Arrays

An array stores multiple elements of the same type in contiguous memory.

```c
int numbers[] = {10, 20, 30, 40};
```

Access elements using zero-based indexing:

```c
printf("%d\n", numbers[0]);  // 10
printf("%d\n", numbers[2]);  // 30
```

The number of elements can be calculated while the array itself is in scope:

```c
int length = sizeof(numbers) / sizeof(numbers[0]);
```

Arrays do not automatically track their length. Accessing an element outside the array's bounds causes undefined behavior.

When an array is passed to a function, the parameter is adjusted to a pointer type. The function does not automatically receive the array's length.

```c
void print_numbers(int *numbers, int length)
{
    for (int i = 0; i < length; i++)
    {
        printf("%d\n", numbers[i]);
    }
}
```

Pass the length separately.

## Pointers

A pointer stores a memory address.

```c
int x = 42;
int *p = &x;
```

- `&x` obtains the address of `x`.
- `int *p` declares a pointer to an integer.
- `*p` dereferences the pointer, accessing the integer at that address.

```c
printf("%d\n", *p);  // 42

*p = 100;

printf("%d\n", x);   // 100
```

Because `p` points to `x`, changing `*p` changes `x`.

### Pointer Arithmetic

For an array:

```c
int numbers[] = {10, 20, 30};
int *p = numbers;

printf("%d\n", *p);       // 10
printf("%d\n", *(p + 1)); // 20
printf("%d\n", p[2]);     // 30

p++;
printf("%d\n", *p);       // 20
```

Pointer arithmetic advances by elements, not individual bytes. Adding `1` to an `int *` advances by `sizeof(int)` bytes.

For an array, `numbers[i]` is equivalent to `*(numbers + i)`.

A pointer can point to an existing object, but dereferencing a null, dangling, or otherwise invalid pointer causes undefined behavior.

### Modifying a Variable Through a Pointer

```c
void change_value(int *p)
{
    *p = 100;
}

int main(void)
{
    int x = 10;
    change_value(&x);

    printf("%d\n", x);  // 100
    return 0;
}
```

The pointer gives the function access to the original variable.

## Strings

C strings are arrays of characters terminated by the null character `'\0'`.

```c
char word[] = "Hello";
```

This array contains six characters in memory:

`'H'`, `'e'`, `'l'`, `'l'`, `'o'`, `'\0'`.

The null terminator tells string-handling functions where the string ends.

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char word[] = "Hello";

    printf("%s\n", word);
    printf("%zu\n", strlen(word));  // 5

    return 0;
}
```

`strlen()` counts characters before the null terminator, not the terminator itself.

When using string functions, ensure the destination array has enough space. Writing beyond its bounds can cause undefined behavior.

## Scope and Lifetime

**Scope** determines where an identifier can be accessed in source code.

**Lifetime** determines how long an object exists during program execution.

```c
void example(void)
{
    int x = 10;
}
```

Here, `x` is a local variable. Its scope is limited to the block, and its lifetime ends when execution leaves that block.

Returning a pointer to an ordinary local variable is unsafe because the variable's lifetime has ended.

```c
int *bad_example(void)
{
    int x = 10;
    return &x;  // Returns a pointer to an object whose lifetime ends
}
```

Do not dereference the returned pointer.

An array declared locally also ceases to exist when its block ends. Copying its elements into an array owned by the caller is different from returning a pointer to the local array.

## Dynamic Memory

Dynamic memory is allocated at runtime using functions from `<stdlib.h>`.

```c
#include <stdlib.h>

int *numbers = malloc(5 * sizeof(*numbers));

if (numbers == NULL)
{
    // Handle allocation failure
}
else
{
    numbers[0] = 10;
    free(numbers);
}
```

- `malloc()` allocates memory without initializing its contents.
- `calloc()` allocates memory and initializes its bytes to zero.
- `realloc()` changes the size of an allocation.
- `free()` releases allocated memory.

Memory allocated dynamically remains allocated until it is freed or the program terminates. Every allocation that is no longer needed should be released appropriately.

Do not access memory after freeing it, free the same allocation twice, or use a pointer after its object has ceased to exist.

## Structures

A `struct` groups related values, potentially of different types.

```c
struct Person
{
    char name[50];
    int age;
};

struct Person person = {"Alice", 25};

printf("%s\n", person.name);
printf("%d\n", person.age);
```

Use `.` to access a member through a structure object and `->` to access a member through a pointer to a structure.

```c
struct Person *p = &person;
printf("%d\n", p->age);
```

## 13. Header Files and Source Files

Header files (`.h`) commonly contain declarations, type definitions, and macros. Source files (`.c`) contain function definitions and other implementation details.

For example, `math_helpers.h`:

```c
int add(int a, int b);
```

And `math_helpers.c`:

```c
#include "math_helpers.h"

int add(int a, int b)
{
    return a + b;
}
```

Compile multiple source files together:

```bash
gcc -Wall -Wextra -Wpedantic main.c math_helpers.c -o program
```

Use header guards to prevent a header from being included multiple times in the same translation unit.

```c
#ifndef MATH_HELPERS_H
#define MATH_HELPERS_H

int add(int a, int b);

#endif
```

## Common Mistakes

- Using `=` instead of `==` in a comparison.
- Forgetting a semicolon after a statement.
- Accessing an array outside its bounds.
- Dereferencing an invalid or uninitialized pointer.
- Using a local variable after its lifetime has ended.
- Forgetting the null terminator in a string.
- Writing beyond an array's capacity.
- Forgetting to free dynamically allocated memory.
- Assuming a function can determine an array's length from a pointer alone.
- Ignoring compiler warnings.

Compile with warnings enabled and treat warnings as opportunities to understand your code better.

## Core Principles to Remember

1. **Types matter.** They determine how values are interpreted and what operations are valid.
2. **Arrays have fixed bounds.** C does not automatically protect you from out-of-bounds access.
3. **Pointers hold addresses.** Dereferencing accesses the object at that address.
4. **Functions receive arguments by value.** Pointers allow functions to access and modify objects owned elsewhere.
5. **Objects have lifetimes.** A valid address must refer to an object that still exists.
6. **Memory management matters.** Know who owns allocated memory and when it must be released.
7. **The compiler is your ally.** Use warnings, read error messages, and test small pieces of code.
8. **Understand before optimizing.** Correct, readable code is a better starting point than clever tricks.
