---
layout: page
title: Dynamic Memory Allocation
description: >
    A chapter about dynamically allocating memory in C.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Dynamic Memory Allocation in C

Dynamic memory allocation allows me to request memory while my program is running, rather than deciding the size of an array entirely in advance.

In C, I use functions from `<stdlib.h>` to allocate and release memory.

### `malloc()`

I use `malloc()` to request a block of memory of a specified size in bytes.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(int));

    if (p == NULL)
    {
        printf("Memory allocation failed.\n");
        return 1;
    }

    *p = 42;
    printf("%d\n", *p);

    free(p);
    return 0;
}
```

- `malloc()` returns a pointer to the beginning of the allocated memory if successful.
- `sizeof(int)` gives me the size of an `int` in bytes.
- The memory returned by `malloc()` is **uninitialized**. I must assign a value before reading it.
- If allocation fails, `malloc()` returns `NULL`.
- I check the returned pointer before accessing the allocated memory.

### `NULL`

`NULL` represents a null pointer, meaning the pointer does not point to a valid object.

I can check whether an allocation succeeded using:

```c
if (p == NULL)
{
    printf("Allocation failed.\n");
    return 1;
}
```

I can also write `if (!p)`. Both forms check whether `p` is a null pointer.

Dereferencing a null pointer, such as `*p`, is undefined behavior. It is not guaranteed to produce a particular result or even crash in a predictable way.

### `free()`

I use `free()` to release dynamically allocated memory when I no longer need it.

```c
int *p = malloc(sizeof(int));

if (p != NULL)
{
    *p = 42;
    free(p);
    p = NULL;
}
```

Important things I learned:

- `free(p)` releases the allocated memory, but it does not destroy the pointer variable itself.
- After `free(p)`, the pointer still contains its old address. It is now a *dangling pointer*.
- I must not dereference a pointer after the memory it points to has been freed.
- Setting `p = NULL` after freeing it is a useful defensive habit, but it does not replace `free()`.
- `free(NULL)` is safe.
- I must not free the same allocation twice or free a pointer that did not come from a compatible memory-allocation function.

### Memory Leaks

A memory leak happens when my program no longer has a usable way to release an allocation that it obtained.

For example:

```c
int *p = malloc(sizeof(int));

if (p != NULL)
{
    p = NULL;  // The allocated memory is now lost.
}
```

Here, I overwrite the only pointer to the allocation without freeing the memory first. I have lost access to that allocation, so I can no longer release it through `p`.

To avoid memory leaks, I need to keep track of allocated memory and ensure it is eventually freed.

### Allocating an Array

I can use `malloc()` to allocate enough memory for an array whose length is determined at runtime.

```c
int length = 5;
int *numbers = malloc(length * sizeof(int));

if (numbers == NULL)
{
    printf("Allocation failed.\n");
    return 1;
}

for (int i = 0; i < length; i++)
{
    numbers[i] = (i + 1) * 10;
}

for (int i = 0; i < length; i++)
{
    printf("%d\n", numbers[i]);
}

free(numbers);
```

I multiply the number of elements by the size of each element:

```c
length * sizeof(int)
```

This gives the number of bytes needed for `length` integers.

Even though `numbers` points to dynamically allocated memory, I can access its elements using array notation, such as `numbers[i]`, or pointer notation, such as `*(numbers + i)`.

### Returning Dynamically Allocated Memory from a Function

A local variable ceases to exist when its function returns. Therefore, I must not return a pointer to an ordinary local variable.

**Incorrect:**

```c
int *create_number(void)
{
    int number = 42;
    return &number;
}
```

The variable `number` no longer exists after `create_number()` returns. The returned pointer is invalid to use.

However, dynamically allocated memory remains allocated until it is released with `free()`.

**Correct:**

```c
#include <stdlib.h>

int *create_number(int value)
{
    int *p = malloc(sizeof(int));

    if (p == NULL)
    {
        return NULL;
    }

    *p = value;
    return p;
}
```

I can then use the function like this:

```c
int *number = create_number(99);

if (number == NULL)
{
    printf("Allocation failed.\n");
    return 1;
}

printf("%d\n", *number);
free(number);
```

The pointer returned by `create_number()` gives the caller access to the allocated integer.

I must remember that:

- The local pointer `p` inside `create_number()` ceases to exist when the function returns.
- The allocated integer remains alive because it has not been freed.
- Returning `p` returns a copy of its pointer value, not a copy of the allocated integer.
- The caller can use that pointer to access the integer.
- The caller is responsible for freeing the allocation when it is no longer needed, according to the function's ownership convention.

### Creating a Dynamically Allocated Array

I can also write a function that allocates an array and returns a pointer to its first element.

```c
#include <stdlib.h>

int *create_array(int length)
{
    if (length < 1)
    {
        return NULL;
    }

    int *p = malloc(length * sizeof(int));

    if (p == NULL)
    {
        return NULL;
    }

    return p;
}
```

The function returns `NULL` if the requested length is invalid or the allocation fails.

The caller can initialize and use the array:

```c
int length = 5;
int *numbers = create_array(length);

if (numbers == NULL)
{
    printf("Could not create array.\n");
    return 1;
}

for (int i = 0; i < length; i++)
{
    numbers[i] = i + 1;
}

free(numbers);
```

The caller knows the array's length, so it can iterate through its elements safely. The pointer alone does not tell me how many elements the allocation contains.

For larger or externally supplied lengths, I should also consider whether the multiplication used to calculate the allocation size could overflow.

### My Mental Model

I find it helpful to distinguish between three things:

1. **The pointer variable:** A variable that stores an address. It has its own lifetime.
2. **The allocated memory:** The block of memory requested using `malloc()`. It remains allocated until it is released.
3. **The pointed-to object:** The value I access by dereferencing a valid pointer, such as `*p`.

A pointer variable and the memory it points to are not the same thing. When a function returns, its local pointer variable disappears, but an allocation it points to can remain alive. Returning the pointer allows the caller to continue accessing that allocation.

### Rules I Want to Remember

- Check whether `malloc()` returned `NULL` before using the allocated memory.
- Initialize allocated memory before reading its contents.
- Keep track of who is responsible for freeing an allocation.
- Free memory when I no longer need it.
- Never dereference a pointer after its allocation has been freed.
- Never return a pointer to an ordinary local variable.
- Remember that a pointer does not automatically tell me the size of the memory it points to.
- Be careful not to lose the only pointer to an allocation before freeing it.

**The main lesson:** A dynamically allocated object can outlive the function that created it, but it remains usable only while the allocation is alive and I have a valid pointer to it.