---
layout: page
title: Strings in C
description: >
    Notes about C strings, character arrays, and working with strings safely.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Strings in C

A string in C is a sequence of characters that ends with a special character called the **null terminator**, written as `'\0'`.

Unlike some other languages, C does not have a built-in string type. I work with strings using arrays of `char` and pointers to characters.

### The Null Terminator

The null terminator marks the end of a C string. It is different from the character `'0'`.

- `'\0'` is the null character, whose value is zero.
- `'0'` is the character representing the digit zero.
- `0` is an integer constant with the value zero.

For example:

```c
char word[] = "hello";
```

C allocates an array containing six elements:

| Index | Value |
|---:|---|
| 0 | `'h'` |
| 1 | `'e'` |
| 2 | `'l'` |
| 3 | `'l'` |
| 4 | `'o'` |
| 5 | `'\0'` |

The five ordinary characters form the word `hello`, and the sixth element marks the end of the string.

I can check the size of the entire array using:

```c
printf("%zu\n", sizeof(word));
```

This prints `6` on a system where `sizeof(char)` is `1`, as C guarantees.

The string has five characters, but the array occupies six bytes because it also stores the null terminator.

### Character Arrays and C Strings

A character array is not automatically a valid C string. For it to be a valid string, it must contain a null terminator within the array's bounds.

For example:

```c
char a[] = "cat";
char b[] = {'c', 'a', 't'};
char c[4] = {'c', 'a', 't'};
```

- `a` is a valid C string. The string literal includes the null terminator.
- `b` is a character array, but it is not a valid C string because it has no null terminator.
- `c` is a valid C string because the remaining element is initialized to zero, which is the value of `'\0'`.

I can also initialize a larger array with a string literal:

```c
char word[10] = "cat";
```

The first four elements are `'c'`, `'a'`, `'t'`, and `'\0'`. The remaining elements are also initialized to zero.

However, an uninitialized local array is different:

```c
char word[10];
```

Its elements have indeterminate values. I must not assume that they contain zeros or any particular characters.

### Printing Strings

I can print a C string using `%s` with `printf()`:

```c
#include <stdio.h>

int main(void)
{
    char word[] = "hello";

    printf("%s\n", word);

    return 0;
}
```

The output is:

```text
hello
```

When `printf()` receives `%s`, it reads characters until it encounters `'\0'`.

This means I must pass a valid, null-terminated string. If an array has no terminator within its bounds, printing it with `%s` can read beyond the array, causing undefined behavior.

C does not automatically check whether a character array contains a valid string.

### Accessing Characters with Indexing and Pointers

I can access characters in a string using array indexing:

```c
char word[] = "hello";

printf("%c\n", word[0]);
printf("%c\n", word[1]);
```

The output is:

```text
h
e
```

I can also use pointer arithmetic:

```c
printf("%c\n", *(word + 0));
printf("%c\n", *(word + 1));
```

These expressions access the same characters as `word[0]` and `word[1]`.

In C, the expression `word[i]` is equivalent to `*(word + i)`.

The index is an offset from the first element. Since array indexing starts at zero, `word[0]` refers to the first character.

### Calculating String Length

I can calculate the length of a string by counting its characters until I reach `'\0'`.

For example:

```c
#include <stdio.h>

int string_length(char *str)
{
    int length = 0;

    while (*str != '\0')
    {
        length++;
        str++;
    }

    return length;
}

int main(void)
{
    char word[] = "hello";

    printf("%d\n", string_length(word));

    return 0;
}
```

The output is:

```text
5
```

Inside the loop:

- `*str` accesses the character currently pointed to.
- `str++` moves the pointer to the next character.
- `length++` increases the count.

The loop stops when the pointer reaches the null terminator. The terminator itself is not counted as part of the string's length.

I could also write the loop using indexing:

```c
int string_length(char *str)
{
    int i = 0;

    while (*(str + i) != '\0')
    {
        i++;
    }

    return i;
}
```

Both approaches count the same characters.

For strings whose lengths might exceed the range of `int`, I should use an appropriate size type such as `size_t`.

### Pointer Subtraction

I can also calculate the length by saving the original pointer and subtracting it from the pointer that reaches the terminator.

```c
#include <stddef.h>

ptrdiff_t string_length(char *str)
{
    char *start = str;

    while (*str != '\0')
    {
        str++;
    }

    return str - start;
}
```

The subtraction gives the distance between the pointers in elements, not bytes.

For example:

```c
char word[] = "hello";
char *p = word;
char *q = word + 5;
```

Here, `q - p` is `5`. The pointer `q` points to the null terminator at index `5`.

Pointer subtraction is valid when both pointers refer to elements of the same array, or one position past its end. The result has type `ptrdiff_t`, defined in `<stddef.h>`, so I can print it using `%td`.

### Copying Strings

I cannot copy the contents of one character array into another by assigning one array to the other.

For example, this is not valid C:

```c
char source[] = "hello";
char destination[10];

destination = source;  // Invalid: arrays cannot be assigned this way.
```

Instead, I need to copy the characters individually or use a suitable string-copying function.

Here is a simple function I wrote to copy a string using pointers:

```c
void string_copy(char *destination, char *source)
{
    while (*source != '\0')
    {
        *destination = *source;

        destination++;
        source++;
    }

    *destination = '\0';
}
```

The function copies each ordinary character and then copies the null terminator.

I can use it like this:

```c
char source[] = "hello";
char destination[10];

string_copy(destination, source);

printf("%s\n", destination);
```

The output is:

```text
hello
```

However, this function assumes that the destination has enough space for the entire string, including its null terminator. It does not check the destination's capacity, so using it with a destination that is too small can cause an out-of-bounds write.

### Copying Strings Safely with a Capacity

To make my copying function safer, I can give it a third parameter that specifies the destination's capacity.

```c
#include <stddef.h>

void string_copy(char *destination, char *source, size_t capacity)
{
    if (capacity == 0)
    {
        return;
    }

    size_t i = 0;

    while (i < capacity - 1 && *source != '\0')
    {
        *destination = *source;

        destination++;
        source++;
        i++;
    }

    *destination = '\0';
}
```

The `capacity` parameter tells the function how many `char` elements the destination can hold.

For example:

```c
char source[] = "peasant";
char destination[4];

string_copy(destination, source, sizeof(destination));

printf("%s\n", destination);
```

The output is:

```text
pea
```

The destination can hold only four elements. Three are used for ordinary characters, and the fourth holds `'\0'`.

The loop condition is:

```c
while (i < capacity - 1 && *source != '\0')
```

It checks two things:

- There is room for another ordinary character while reserving one element for the terminator.
- The source string has not ended.

After the loop, I write `'\0'` at the current destination position. This works whether the source ends early or the destination runs out of room first.

If `capacity` is zero, the function returns immediately because there is no destination element that it can safely write to. This check must happen before evaluating `capacity - 1`, because `size_t` is an unsigned type.

I also use `size_t` for the counter because it is a type intended for representing sizes and array capacities.

There are still assumptions: the source must point to a valid null-terminated string, the destination must point to writable memory of at least the stated capacity, and the two regions must not overlap.

### Understanding Capacity

The capacity includes the space needed for the null terminator. It is not the maximum number of ordinary characters.

| Destination capacity | Maximum ordinary characters | Required terminator |
|---:|---:|---|
| 1 | 0 | Index 0 |
| 4 | 3 | Index 3 |
| 7 | 6 | Index 6 |

For example:

```c
char a[1];
char b[4];
char c[10];
```

If I copy `"cat"` into these arrays using the bounded function:

- `a` becomes an empty string, containing only `'\0'`.
- `b` contains `"cat"` followed by `'\0'`.
- `c` contains `"cat"` followed by `'\0'`, with six additional zero-initialized elements only if the array was initialized that way. In this copying example, the elements after the terminator are not necessarily initialized.

The last distinction matters: my copying function writes only the characters and the terminator. It does not clear the rest of the destination array.

### My Mental Model

I find it helpful to remember three things:

1. **The character array:** The memory that holds the characters.
2. **The string:** The sequence of characters up to, but not including, the first `'\0'`.
3. **The null terminator:** The marker that tells string-handling functions where the string ends.

The array may contain more elements than the string needs. Those extra elements are part of the array, but they are not part of the string after its first null terminator.

A pointer to the first character does not automatically tell me how large the array is or whether the characters form a valid string. I need to keep track of the available capacity and ensure the terminator is present.

### Rules I Want to Remember

- A C string ends with `'\0'`.
- The null terminator occupies an element in the character array but is not counted in the string's length.
- A character array is not necessarily a valid C string.
- `sizeof(array)` gives the size of the array in bytes when the actual array type is in scope; it does not give the string's length.
- `word[i]` is equivalent to `*(word + i)`.
- Pointer subtraction measures the distance in elements, not bytes.
- I cannot assign one array to another using `=`.
- When copying a string, I must make sure the destination has enough space.
- If I know the destination's capacity, I can reserve one element for the null terminator and avoid writing beyond that capacity.
- An initialized character array can contain extra zero-valued elements, but an uninitialized local array has indeterminate contents.
- I must not assume that `printf("%s", array)` is safe unless the array contains a valid null-terminated string.

**The main lesson:** A C string is a sequence of characters ending with `'\0'`. I need to understand both the string's length and the capacity of the array that stores it so I can work with strings safely.