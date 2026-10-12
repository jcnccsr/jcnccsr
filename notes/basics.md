---
layout: page
title: C Basics
description: >
    Notes I keep for myself while learning the basics of C.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What I Know About C So Far

C is a compiled, general-purpose programming language. I write a program as source code, compile it into machine code, and then run it.

Here is a small program I can use as a starting point:

```c
#include <stdio.h>

int main(void)
{
    printf("Hello, world!\n");
    return 0;
}
```

- I use `#include <stdio.h>` to include declarations for standard input/output functions such as `printf()`.
- `int main(void)` is the program's entry point.
- I use `{}` to enclose a block of code.
- `printf()` prints formatted output.
- `\n` represents a newline.
- `return 0;` indicates that the program finished successfully.
- I end a statement with `;`.

### How I Compile and Run It

With GCC, I can compile and run it like this:

```bash
gcc main.c -o main
./main
```

I can ask GCC to show me useful warnings with these options:

```bash
gcc -Wall -Wextra -Wpedantic main.c -o main
```

## Variables and Data Types I Need to Remember

I use a variable to store a value of a particular type.

```c
int age = 42;
float temperature = 36.5f;
double pi = 3.1415926535;
char letter = 'A';
```
These are some common types I will run into:

| Type | Purpose |
|---|---|
| `char` | Character or small integer |
| `int` | Integer |
| `float` | Floating-point number |
| `double` | Floating-point number with greater precision than `float` |
| `void` | No value or unspecified type, depending on context |

I should not assume every type has exactly the same size and range on every implementation.

When I do not want to change a value, I can declare it with `const`:

```c
const int DAYS_IN_WEEK = 7;
```

## Operators I Have Seen

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

I need to remember that integer division discards the fractional part: `10 / 3` produces `3`.

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

In C, I can treat `0` as false and nonzero values as true.

## Conditional Statements

I use `if`, `else if`, and `else` to choose which code executes.

```c
int age = 42;

if (age >= 18)
{
    printf("Adult\n");
}
else
{
    printf("Minor\n");
}
```

I can use `switch` to select a branch based on an integer or compatible expression.

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

I use `break` when I want to stop a case from falling through into the next one.

## Loops

I use loops when I want to repeat code.

### `for`

I find `for` useful when I know the iteration structure.

```c
for (int i = 0; i < 5; i++)
{
    printf("%d\n", i);
}
```

### `while`

`while` repeats as long as its condition is true.

```c
int i = 0;

while (i < 5)
{
    printf("%d\n", i);
    i++;
}
```

### `do-while`

`do-while` runs its body at least once.

```c
int i = 0;

do
{
    printf("%d\n", i);
    i++;
}
while (i < 5);
```

I can use `break` to exit a loop and `continue` to skip to its next iteration.

## Functions

I use functions to organize code into reusable units.

```c
int add(int a, int b)
{
    return a + b;
}
```

I can call the function like this:

```c
int result = add(3, 4);
```

A function declaration, also called a prototype, tells the compiler about a function before I define it.

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

I need to remember that C passes function arguments by value, so a function receives copies of the argument values. If I want a function to modify a variable owned by its caller, I can pass it a pointer to that variable.

## Arrays

I use an array to store multiple values of the same type in contiguous memory.

```c
int numbers[] = {10, 20, 30, 40};
```

I access array elements with zero-based indexing:

```c
printf("%d\n", numbers[0]);  // 10
printf("%d\n", numbers[2]);  // 30
```

While the array itself is in scope, I can calculate its number of elements like this:

```c
int length = sizeof(numbers) / sizeof(numbers[0]);
```

I need to keep track of an array's length myself. Accessing an element outside its bounds causes undefined behavior.

When I pass an array to a function, its parameter is adjusted to a pointer type. The function does not automatically receive the array's length, so I need to pass that separately.

```c
void print_numbers(int *numbers, int length)
{
    for (int i = 0; i < length; i++)
    {
        printf("%d\n", numbers[i]);
    }
}
```

I pass the length separately.

## Pointers

I think of a pointer as a value that stores a memory address.

```c
int x = 42;
int *p = &x;
```

- `&x` gives me the address of `x`.
- `int *p` declares a pointer to an integer.
- `*p` dereferences the pointer, letting me access the integer at that address.

```c
printf("%d\n", *p);  // 42

*p = 100;

printf("%d\n", x);   // 100
```

Because `p` points to `x`, changing `*p` changes `x` too.

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

I need to remember that pointer arithmetic advances by elements, not individual bytes. Adding `1` to an `int *` advances by `sizeof(int)` bytes.

For an array, `numbers[i]` is equivalent to `*(numbers + i)`.

I should only dereference a pointer when it points to a valid object. Dereferencing a null, dangling, or otherwise invalid pointer causes undefined behavior.

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

Here, the pointer gives the function access to the original variable.

## Strings

I need to remember that C strings are arrays of characters terminated by the null character `'\0'`.

```c
char word[] = "Hello";
```

This array contains six characters in memory:

`'H'`, `'e'`, `'l'`, `'l'`, `'o'`, `'\0'`.

The null terminator tells string-handling functions where my string ends.

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

I use `strlen()` to count the characters before the null terminator; it does not count the terminator itself.

When I use string functions, I need to make sure the destination array has enough space. Writing beyond its bounds can cause undefined behavior.

## Scope and Lifetime

I use **scope** to think about where an identifier can be accessed in source code.

An object's **lifetime** is how long it exists while my program runs.

```c
void example(void)
{
    int x = 10;
}
```

Here, `x` is local to the block, and its lifetime ends when execution leaves that block.

I must not return a pointer to an ordinary local variable: by the time the caller uses it, that variable's lifetime has ended.

```c
int *bad_example(void)
{
    int x = 10;
    return &x;  // Returns a pointer to an object whose lifetime ends
}
```

I should not dereference the returned pointer.

A local array also ceases to exist when its block ends. Copying its elements into an array owned by the caller is different from returning a pointer to that local array.

## Dynamic Memory

I can allocate dynamic memory at runtime using functions from `<stdlib.h>`.

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

- I use `malloc()` to allocate memory without initializing its contents.
- I use `calloc()` to allocate memory and initialize its bytes to zero.
- I use `realloc()` to change the size of an allocation.
- I use `free()` to release allocated memory.

Memory I allocate dynamically remains allocated until I free it or the program terminates. I need to release allocations when I no longer need them.

I must not access memory after freeing it, free the same allocation twice, or use a pointer after its object has ceased to exist.

## Structures

I use a `struct` to group related values, including values of different types.

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

I use `.` to access a member through a structure object and `->` to access one through a pointer to a structure.

```c
struct Person *p = &person;
printf("%d\n", p->age);
```

## Header Files and Source Files

I usually put declarations, type definitions, and macros in header files (`.h`). Source files (`.c`) hold function definitions and other implementation details.

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

I can compile multiple source files together:

```bash
gcc -Wall -Wextra -Wpedantic main.c math_helpers.c -o program
```

I use header guards to prevent a header from being included multiple times in the same translation unit.

```c
#ifndef MATH_HELPERS_H
#define MATH_HELPERS_H

int add(int a, int b);

#endif
```

## Mistakes I Want to Watch For

- I might use `=` instead of `==` in a comparison.
- I might forget a semicolon after a statement.
- I might access an array outside its bounds.
- I might dereference an invalid or uninitialized pointer.
- I might use a local variable after its lifetime has ended.
- I might forget the null terminator in a string.
- I might write beyond an array's capacity.
- I might forget to free dynamically allocated memory.
- I might assume a function can determine an array's length from a pointer alone.
- I might ignore compiler warnings instead of learning from them.

I want to compile with warnings enabled and use them as chances to understand my code better.

## Reminders to Myself

1. I need to pay attention to **types** because they determine how values are interpreted and which operations are valid.
2. I need to keep track of **array bounds**; C does not automatically protect me from out-of-bounds access.
3. **Pointers hold addresses.** Dereferencing one lets me access the object at that address.
4. **Functions receive arguments by value.** Pointers let a function access and modify objects owned elsewhere.
5. **Objects have lifetimes.** An address is only useful while the object it refers to still exists.
6. I need to know who owns allocated memory and when I should release it.
7. I can use compiler warnings, error messages, and small tests to help me understand what is happening.
8. I should aim for correct, readable code before trying to optimize it.
