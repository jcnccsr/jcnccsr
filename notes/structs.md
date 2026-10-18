---
layout: page
title: Structs in C
description: >
    Notes about structures, members, and working with structs and pointers in C.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Structs in C

A `struct` allows me to group related pieces of data under one type. The data can have different types, but they belong together logically.

For example, I might want to represent a person using a name and an age. Instead of storing these separately, I can put them in a structure.

### Defining a Struct

I define a structure using the `struct` keyword.

```c
struct Person
{
    char name[20];
    int age;
};
```

Here, `struct Person` describes a type of structure with two members:

- `name`, a character array that can hold up to 19 ordinary characters and a null terminator.
- `age`, an integer.

The definition describes the structure, but it does not create a variable named `Person` or a person named Alice. It establishes the layout that variables of this type will use.

I can create variables of this type:

```c
struct Person alice;
struct Person bob;
```

Each variable has its own storage for its members. Changing `alice.age` does not automatically change `bob.age`.

```c
alice.age = 30;
bob.age = 25;
```

The two variables are separate objects, even though they have the same structure type.

### Initializing a Struct

I can initialize a structure when I declare it:

```c
struct Person alice = {"Alice", 30};
```

The values are assigned to the members in the order in which they are declared.

I can also use designated initializers to make it clear which member receives each value:

```c
struct Person alice = {
    .name = "Alice",
    .age = 30
};
```

This is useful when a structure has several members or when I want the initialization to be easier to read.

I can initialize all members to zero using:

```c
struct Person alice = {0};
```

For this structure, `alice.age` becomes `0`, and every element of `alice.name` is initialized to zero.

### Accessing Struct Members

I use the dot operator (`.`) to access a member of a structure variable.

```c
struct Person alice = {"Alice", 30};

printf("%s is %d years old.\n", alice.name, alice.age);
```

I can also change a member:

```c
alice.age = 31;
```

The dot operator lets me access a particular member of a particular structure object.

### Character Arrays Inside Structs

My `name` member is an array of characters, so the same string rules apply to it.

```c
struct Person alice = {"Alice", 30};
```

The name is initialized with a string literal, including its null terminator.

However, I cannot assign a new string to the array after the structure has already been declared:

```c
alice.name = "Chad";  // Invalid C
```

This is because arrays cannot be assigned to using `=`.

If I initialize the structure with zeros, I can fill the name character by character:

```c
struct Person alice = {0};

alice.name[0] = 'C';
alice.name[1] = 'h';
alice.name[2] = 'a';
alice.name[3] = 'd';
```

The remaining elements are already zero, so `alice.name[4]` contains `'\0'`. The name is therefore a valid C string.

I need to remember that the array has a fixed capacity. If I fill every element with ordinary characters and do not leave room for `'\0'`, it will not be a valid C string.

### Struct Size and Padding

I can use `sizeof` to find the size of a structure in bytes.

```c
printf("%zu\n", sizeof(struct Person));
```

I might expect the size of `struct Person` to be the size of `name` plus the size of `age`. However, the compiler may insert extra bytes called **padding**.

Padding can help the compiler arrange members in memory to satisfy alignment requirements.

For example, a structure containing a `char` followed by an `int` might have padding between the members.

```c
struct Example
{
    char letter;
    int number;
};
```

The actual size depends on the implementation and its alignment requirements. I should not assume that the size of a structure is always the sum of the sizes of its members.

### Passing a Struct to a Function

When I pass a structure to a function by value, the function receives its own copy of the structure.

```c
#include <stdio.h>

struct Person
{
    char name[20];
    int age;
};

void print_person(struct Person person)
{
    printf("%s is %d years old.\n", person.name, person.age);
}

int main(void)
{
    struct Person alice = {"Alice", 30};

    print_person(alice);

    return 0;
}
```

The function receives a copy of `alice`. It can read the members without modifying the original structure.

For example:

```c
void make_older(struct Person person)
{
    person.age = 99;
}
```

If I call `make_older(alice)`, the function changes the age in its local copy. The original `alice.age` in `main()` remains `30`.

This is because the function parameter is a separate structure object.

### Passing a Pointer to a Struct

If I want a function to modify the original structure, I can pass a pointer to it.

```c
void make_older(struct Person *person)
{
    person->age++;
}
```

I can call the function using the address-of operator:

```c
struct Person alice = {"Alice", 30};

make_older(&alice);
```

The function receives the address of `alice`, so it can access and modify the original structure through that pointer.

After the call, `alice.age` is `31`.

### The Dot Operator and the Arrow Operator

I use the dot operator when I have a structure object:

```c
alice.age
```

I use the arrow operator (`->`) when I have a pointer to a structure:

```c
person->age
```

The arrow operator is shorthand for dereferencing the pointer and then accessing a member.

These expressions are equivalent:

```c
person->age
(*person).age
```

The parentheses in `(*person).age` are necessary because the dot operator has higher precedence than the unary `*` operator.

I find it helpful to remember:

- `alice.age` means I have a structure object.
- `person->age` means I have a pointer to a structure object.

### Reassigning a Pointer Versus Modifying a Struct

This distinction took me a little thought to understand.

Consider this function:

```c
void change_person(struct Person *person)
{
    struct Person copy = {"Bob", 40};

    person = &copy;
}
```

The assignment:

```c
person = &copy;
```

changes the local pointer variable `person`. It makes that pointer point to `copy` instead of the original structure.

It does not change which object the caller's pointer variable points to. The caller's original structure remains unchanged by this pointer reassignment.

Now consider a different assignment:

```c
void change_person(struct Person *person)
{
    struct Person copy = {"Bob", 40};

    *person = copy;
}
```

This time, `*person` means the structure object that the pointer points to.

The assignment:

```c
*person = copy;
```

copies the contents of `copy` into the structure pointed to by `person`. If `person` points to `alice`, then `alice` receives Bob's name and age.

I want to remember the difference:

- `person = &copy;` changes the local pointer's stored address.
- `*person = copy;` assigns the contents of one structure to another structure.

The second assignment is valid because structures can be assigned to each other when they have compatible structure types. This differs from arrays, which cannot be assigned using `=`.

### Copying a Struct

I can assign one structure to another structure of the same type.

```c
struct Person alice = {"Alice", 30};
struct Person bob;

bob = alice;
```

After the assignment, `bob` has the same member values as `alice`.

Because the structure contains a character array, the array elements are copied as part of the structure assignment. I do not need to copy the name character by character in this case.

However, this is a value copy, not a permanent connection between the two structures. Changing `bob.age` afterward does not change `alice.age`.

If a structure contains pointers, assigning the structure copies the pointer values too. It does not automatically duplicate the memory those pointers refer to. I need to be careful about that when working with dynamically allocated memory.

### Working with an Array of Structs

I can create an array of structures when I want to store several objects of the same type.

```c
struct Person people[] = {
    {"Alice", 30},
    {"Bob", 25},
    {"Charlie", 35}
};
```

Each element is a separate `struct Person`.

I can access a member of an element using both array indexing and the dot operator:

```c
printf("%s\n", people[0].name);
printf("%d\n", people[1].age);
```

I can loop through the array:

```c
size_t length = sizeof(people) / sizeof(people[0]);

for (size_t i = 0; i < length; i++)
{
    printf("%s is %d years old.\n",
           people[i].name, people[i].age);
}
```

Here, `sizeof(people)` gives the size of the entire array, while `sizeof(people[0])` gives the size of one structure element. Dividing the two gives the number of elements.

This calculation works here because `people` is an actual array in this scope. If I pass the array to a function as a pointer, `sizeof` on that pointer will not give me the array's length.

### Finding the Oldest Person Using a Pointer

I can also use pointers to traverse an array of structures.

```c
struct Person *oldest = people;

for (size_t i = 1; i < length; i++)
{
    if (people[i].age > oldest->age)
    {
        oldest = &people[i];
    }
}
```

The pointer `oldest` initially points to the first element of the array.

Whenever I find someone older, I update `oldest` to point to that person's structure. At the end, it points to the oldest person in the array.

I use `oldest->age` because `oldest` is a pointer. I use `people[i].age` because `people[i]` is a structure object.

There is an important difference between these two assignments:

```c
oldest = &people[i];
```

This changes which structure `oldest` points to.

```c
*oldest = people[i];
```

This copies the contents of `people[i]` into the structure that `oldest` currently points to. It does not merely change the pointer.

### My Mental Model

I find it helpful to distinguish between three things:

1. **The struct type:** The definition that describes the members a structure contains.
2. **The struct object:** An actual variable, such as `alice`, that has storage for those members.
3. **The pointer to a struct:** A variable that stores the address of a structure object.

The dot operator accesses members of a structure object. The arrow operator accesses members through a pointer to a structure.

Passing a structure by value gives a function a copy. Passing a pointer gives the function a way to access the original object, provided the pointer is valid and the function uses it to modify that object.

### Rules I Want to Remember

- A `struct` groups related data under one type.
- Each structure variable has its own members and storage.
- I use `.` to access members of a structure object.
- I use `->` to access members through a pointer.
- `p->age` is equivalent to `(*p).age`.
- Structure members can include arrays, pointers, and other structures.
- I cannot assign a new string to a character array member using `=`.
- Structure size can include padding for alignment.
- Passing a structure by value gives the function a copy.
- Passing a pointer allows a function to access the original structure.
- `p = &copy` changes a local pointer; `*p = copy` assigns a structure to the pointed-to object.
- Structure assignment copies member values, including array members, but does not automatically duplicate dynamically allocated memory referenced by pointer members.
- A pointer to a structure does not automatically tell me how many structures are in an array.

**The main lesson:** A structure lets me organize related data into one object. Understanding the difference between the structure, its members, and pointers to it helps me pass data to functions, modify objects, and work with collections of structures.