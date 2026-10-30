---
layout: page
title: JavaScript Basics
description: >
    Notes I keep for myself while learning JavaScript and building interactive web applications.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What I Know About JavaScript So Far

JavaScript is a high-level, general-purpose programming language. It is widely used to make websites interactive and to build web applications.

Unlike C, which I compile into an executable before running it, JavaScript is commonly executed directly by a JavaScript engine inside a browser.

I can use JavaScript for several purposes:

- Manipulating HTML elements.
- Responding to user interactions.
- Performing calculations and processing data.
- Communicating with servers.
- Building interactive applications.
- Running backend applications using Node.js.

For my Cursus project, I will primarily use JavaScript in the browser.

I want to build a **Single-Page Application (SPA)**: an application that loads its main page once and updates its content dynamically without requiring a full page reload for every navigation.

I can build this using vanilla JavaScript, without a framework.

## 1. Variables and Data Types

I use variables to store information.

JavaScript has three ways to declare variables:

```javascript
let username = "Peasant";
const level = 1;
var oldVariable = "I should generally avoid var";
```

I should primarily use `const` and `let`.

- `const` declares a variable that cannot be reassigned.
- `let` declares a variable that can be reassigned.
- `var` is an older declaration mechanism with different scoping rules.

Example:

```javascript
let experience = 0;

experience = 100;

const maxLevel = 100;
```

I cannot reassign `maxLevel`.

However, `const` does not make an object or array immutable. It only prevents me from assigning a different value to that variable.

### Basic Data Types

JavaScript has several primitive types:

| Type | Example | Purpose |
|---|---|---|
| `string` | `"Hello"` | Text |
| `number` | `42` | Numbers, including integers and decimals |
| `boolean` | `true` | True or false |
| `undefined` | `undefined` | A value has not been assigned |
| `null` | `null` | An intentional absence of value |
| `bigint` | `42n` | Large integers |
| `symbol` | `Symbol()` | Unique identifiers |

JavaScript also has objects, which include arrays and functions.

I can use `typeof` to inspect a value's type:

```javascript
let username = "Alice";
let age = 25;
let active = true;

console.log(typeof username); // string
console.log(typeof age);      // number
console.log(typeof active);   // boolean
```

One unusual detail: `typeof null` returns `"object"`. This is an old JavaScript behavior.

## 2. Operators

### Arithmetic

```javascript
let a = 10;
let b = 3;

console.log(a + b); // 13
console.log(a - b); // 7
console.log(a * b); // 30
console.log(a / b); // 3.333...
console.log(a % b); // 1
console.log(a ** b); // 1000
```

### Comparison

```javascript
a === b; // Strict equality
a !== b; // Strict inequality
a > b;
a < b;
a >= b;
a <= b;
```

I should generally use `===` and `!==` rather than `==` and `!=`.

The strict comparison operators do not perform automatic type conversion.

```javascript
5 == "5";  // true
5 === "5"; // false
```

This is important because JavaScript sometimes converts values automatically when using non-strict comparisons.

### Logical Operators

```javascript
let loggedIn = true;
let hasPermission = true;

loggedIn && hasPermission; // AND
loggedIn || hasPermission; // OR
!loggedIn;                 // NOT
```

I can combine conditions:

```javascript
if (loggedIn && hasPermission) {
    console.log("Access granted");
}
```

### Assignment Operators

```javascript
let experience = 100;

experience += 50; // 150
experience -= 20; // 130
experience *= 2;  // 260
experience /= 2;  // 130
```

### Increment and Decrement

```javascript
let level = 1;

level++;
level--;

console.log(level);
```

## 3. Conditional Statements

I use conditions to decide which code should execute.

```javascript
let experience = 150;

if (experience >= 100) {
    console.log("Level up!");
} else {
    console.log("Keep training!");
}
```

I can use `else if` to check multiple conditions:

```javascript
let level = 15;

if (level >= 50) {
    console.log("Master");
} else if (level >= 20) {
    console.log("Experienced");
} else {
    console.log("Beginner");
}
```

### Switch

I can use `switch` when I want to compare one value against several possibilities.

```javascript
let rank = "silver";

switch (rank) {
    case "gold":
        console.log("Gold rank");
        break;

    case "silver":
        console.log("Silver rank");
        break;

    default:
        console.log("Unranked");
}
```

I use `break` to prevent execution from continuing into the next case.

### Ternary Operator

For a simple conditional expression, I can use the ternary operator:

```javascript
let loggedIn = true;

let message = loggedIn ? "Welcome" : "Please log in";
```

I should avoid making complicated nested ternary expressions because they can become difficult to read.

## 4. Loops

Loops allow me to repeat code.

### for

```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

### while

```javascript
let i = 0;

while (i < 5) {
    console.log(i);
    i++;
}
```

### for...of

I can use `for...of` to iterate over the values of an array:

```javascript
const skills = ["C", "Pointers", "Structures"];

for (const skill of skills) {
    console.log(skill);
}
```

### for...in

I can use `for...in` to iterate over the enumerable property names of an object:

```javascript
const player = {
    username: "Alice",
    level: 10
};

for (const property in player) {
    console.log(property);
}
```

I should remember the distinction:

- `for...of` iterates over values in an iterable, such as an array.
- `for...in` iterates over enumerable property keys.

For arrays, I will generally prefer `for...of` or array methods.

## 5. Functions

Functions allow me to organize code into reusable units.

```javascript
function add(a, b) {
    return a + b;
}

const result = add(3, 4);

console.log(result); // 7
```

JavaScript also supports function expressions:

```javascript
const multiply = function(a, b) {
    return a * b;
};
```

And arrow functions:

```javascript
const subtract = (a, b) => {
    return a - b;
};
```

For a function containing only one expression, I can omit the braces and `return`:

```javascript
const double = number => number * 2;
```

I will encounter arrow functions frequently when working with arrays, events, and asynchronous operations.

### Parameters and Default Values

```javascript
function createPlayer(username, level = 1) {
    return {
        username: username,
        level: level
    };
}

const player = createPlayer("Alice");
```

The default value is used when the second argument is omitted or `undefined` is passed.

### Returning Objects

Functions can return objects containing several related values.

```javascript
function createCharacter(name, level) {
    return {
        name: name,
        level: level,
        experience: 0
    };
}

const character = createCharacter("Alice", 1);
```

This will be useful when creating player profiles, skills, quests, and achievements.

## 6. Arrays

Arrays allow me to store multiple values in one variable.

```javascript
const skills = ["C", "Pointers", "Structures"];

console.log(skills[0]); // C
console.log(skills[1]); // Pointers
```

Array indexes begin at zero.

I can access the number of elements with `.length`:

```javascript
console.log(skills.length); // 3
```

### Adding and Removing Elements

```javascript
const inventory = [];

inventory.push("Sword");
inventory.push("Shield");

inventory.pop();

console.log(inventory);
```

- `push()` adds an element to the end.
- `pop()` removes the last element.
- `unshift()` adds an element to the beginning.
- `shift()` removes the first element.

### Useful Array Methods

#### map()

Creates a new array by transforming every element.

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(doubled); // [2, 4, 6]
```

#### filter()

Creates a new array containing elements that satisfy a condition.

```javascript
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter(number => number % 2 === 0);

console.log(evenNumbers); // [2, 4]
```

#### find()

Returns the first element that satisfies a condition.

```javascript
const quests = [
    { id: 1, name: "Learn C" },
    { id: 2, name: "Learn Pointers" }
];

const quest = quests.find(quest => quest.id === 2);
```

#### some()

Checks whether at least one element satisfies a condition.

```javascript
const numbers = [1, 3, 5, 8];

const hasEvenNumber = numbers.some(number => number % 2 === 0);
```

#### every()

Checks whether all elements satisfy a condition.

```javascript
const numbers = [2, 4, 6];

const allEven = numbers.every(number => number % 2 === 0);
```

#### reduce()

Combines array elements into a single value.

```javascript
const experience = [100, 200, 150];

const total = experience.reduce(
    (sum, amount) => sum + amount,
    0
);

console.log(total); // 450
```

I should learn these methods because they are very common when processing collections of data.

## 7. Objects

Objects are one of the most important concepts in JavaScript.

An object groups related information into properties and can also contain functions called methods.

```javascript
const player = {
    username: "Alice",
    level: 5,
    experience: 250,

    gainExperience(amount) {
        this.experience += amount;
    }
};
```

I can access properties using dot notation:

```javascript
console.log(player.username);
console.log(player.level);
```

Or bracket notation:

```javascript
console.log(player["username"]);
```

I can modify properties:

```javascript
player.level = 6;
player.experience = 300;
```

I can call an object's methods:

```javascript
player.gainExperience(100);
```

### Nested Objects

Objects can contain other objects:

```javascript
const player = {
    username: "Alice",

    character: {
        level: 5,
        experience: 250
    },

    statistics: {
        completedQuests: 12,
        achievements: 4
    }
};

console.log(player.character.level);
console.log(player.statistics.completedQuests);
```

This is useful when representing complex information.

For example, a player could have a character, an inventory, statistics, and achievements.

### Arrays of Objects

This is particularly important for my Cursus project.

```javascript
const quests = [
    {
        id: 1,
        title: "Introduction to C",
        difficulty: "Easy",
        completed: true
    },
    {
        id: 2,
        title: "Understanding Pointers",
        difficulty: "Medium",
        completed: false
    },
    {
        id: 3,
        title: "Dynamic Memory",
        difficulty: "Hard",
        completed: false
    }
];
```

I can retrieve a particular quest:

```javascript
const quest = quests.find(quest => quest.id === 2);

console.log(quest.title);
```

I can filter completed quests:

```javascript
const completedQuests = quests.filter(quest => quest.completed);
```

This is how I can represent collections of skills, projects, quests, and achievements.

## 8. Destructuring and Spread Syntax

These are useful features for working with objects and arrays.

### Object Destructuring

Instead of accessing each property individually:

```javascript
const player = {
    username: "Alice",
    level: 10
};

const username = player.username;
const level = player.level;
```

I can write:

```javascript
const { username, level } = player;
```

### Array Destructuring

```javascript
const skills = ["C", "JavaScript", "SQL"];

const [first, second, third] = skills;
```

### Spread Syntax

I can use `...` to copy or combine arrays and objects.

```javascript
const original = [1, 2, 3];

const copy = [...original];
```

Combining arrays:

```javascript
const first = [1, 2];
const second = [3, 4];

const combined = [...first, ...second];
```

Copying and modifying an object:

```javascript
const player = {
    username: "Alice",
    level: 5
};

const updatedPlayer = {
    ...player,
    level: 6
};
```

This is particularly useful because it lets me create updated objects without directly modifying the original object.

I should remember that spread syntax creates a shallow copy, not a deep copy.

## 9. Scope and Closures

Scope determines where a variable can be accessed.

JavaScript has global scope, function scope, and block scope.

```javascript
const globalMessage = "Hello";

function example() {
    const localMessage = "World";

    console.log(globalMessage);
    console.log(localMessage);
}
```

The variable `localMessage` cannot be accessed outside the function.

Variables declared with `let` and `const` are block-scoped:

```javascript
if (true) {
    let level = 10;
}

console.log(level); // ReferenceError
```

### Closures

A closure happens when a function retains access to variables from the scope in which it was created, even after that outer function has finished executing.

```javascript
function createCounter() {
    let count = 0;

    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

The returned function remembers the `count` variable.

Closures are useful for keeping certain data private and creating functions that maintain their own state.

I do not need to master every detail immediately, but I should understand the concept.

## 10. Classes and Constructors

JavaScript supports classes, which provide syntax for creating objects with shared behavior.

```javascript
class Player {
    constructor(username) {
        this.username = username;
        this.level = 1;
        this.experience = 0;
    }

    gainExperience(amount) {
        this.experience += amount;
    }
}

const player = new Player("Alice");

player.gainExperience(100);

console.log(player.experience);
```

- `class` defines a class.
- `constructor()` initializes a new object.
- `this` refers to the current instance.
- `new` creates an instance of the class.

I can also extend a class:

```javascript
class Character {
    constructor(name) {
        this.name = name;
    }
}

class Warrior extends Character {
    constructor(name) {
        super(name);
        this.className = "Warrior";
    }
}
```

I should understand classes, but I do not need to use them everywhere. Plain objects and functions are often sufficient.

## 11. The DOM

The **Document Object Model (DOM)** is the browser's representation of an HTML document as a tree of objects.

JavaScript can interact with this tree to change what appears on a webpage.

For example, consider this HTML:

```html
<h1 id="title">Welcome</h1>
```

I can access the element using JavaScript:

```javascript
const title = document.getElementById("title");

title.textContent = "Welcome, Adventurer!";
```

### Selecting Elements

Some useful methods:

```javascript
document.getElementById("title");

document.querySelector(".card");

document.querySelectorAll(".card");

document.createElement("div");
```

- `getElementById()` selects an element by ID.
- `querySelector()` selects the first matching element.
- `querySelectorAll()` selects all matching elements.
- `createElement()` creates a new HTML element.

### Modifying Elements

```javascript
const title = document.querySelector("#title");

title.textContent = "My Cursus";

title.classList.add("active");

title.classList.remove("hidden");

title.classList.toggle("completed");
```

I can also change an element's attributes:

```javascript
const image = document.querySelector("#avatar");

image.setAttribute("src", "avatar.png");
```

### Creating Elements

I can dynamically create HTML elements:

```javascript
const list = document.querySelector("#quest-list");

const item = document.createElement("li");

item.textContent = "Learn pointers";

list.append(item);
```

This is important because my application will need to display quests, skills, achievements, and other information dynamically.

When displaying user-provided text, I should prefer `textContent` rather than inserting it with `innerHTML`. Inserting untrusted HTML can create cross-site scripting vulnerabilities.

## 12. Events

Events allow JavaScript to react to user interactions.

Some common events include:

| Event | When it happens |
|---|---|
| `click` | User clicks an element |
| `submit` | User submits a form |
| `input` | User changes an input value |
| `change` | An input's value is committed |
| `keydown` | User presses a keyboard key |
| `mouseover` | Pointer moves over an element |

I can listen for an event using `addEventListener()`.

```javascript
const button = document.querySelector("#my-button");

button.addEventListener("click", function() {
    console.log("Button clicked!");
});
```

Using an arrow function:

```javascript
button.addEventListener("click", () => {
    console.log("Button clicked!");
});
```

### Event Objects

An event listener receives an event object containing information about the event.

```javascript
button.addEventListener("click", (event) => {
    console.log(event.target);
});
```

- `event.target` identifies the element where the event originated.
- `event.currentTarget` identifies the element whose listener is currently executing.

### Event Delegation

Event delegation allows me to listen for events on a parent element instead of adding a separate listener to every child.

```javascript
const questList = document.querySelector("#quest-list");

questList.addEventListener("click", (event) => {
    const button = event.target.closest("[data-quest-id]");

    if (!button || !questList.contains(button)) {
        return;
    }

    const questId = button.dataset.questId;

    console.log("Selected quest:", questId);
});
```

This is useful when elements are created dynamically.

For example, I might have hundreds of quests. Instead of attaching a listener to every quest, I can attach one listener to their parent container.

## 13. Forms and User Input

My application will eventually need forms for creating accounts, editing profiles, and submitting project results.

I can retrieve the value of an input:

```html
<input id="username" type="text">
<button id="submit">Submit</button>
```

```javascript
const usernameInput = document.querySelector("#username");
const submitButton = document.querySelector("#submit");

submitButton.addEventListener("click", () => {
    const username = usernameInput.value;

    console.log(username);
});
```

For forms, I should listen to the `submit` event:

```javascript
const form = document.querySelector("#login-form");

form.addEventListener("submit", (event) => {
    event.preventDefault();

    const username = document.querySelector("#username").value;

    console.log(username);
});
```

`preventDefault()` prevents the browser's normal form submission behavior.

I can use HTML attributes such as `required`, `minlength`, and `maxlength` for basic validation.

However, important validation must also happen on the server. Browser-side validation can be bypassed.

## 14. JSON

JSON stands for JavaScript Object Notation.

It is a text format commonly used to exchange data between applications.

For example:

```json
{
    "username": "Alice",
    "level": 5,
    "experience": 250,
    "skills": ["C", "Pointers", "Structures"]
}
```

JSON looks similar to JavaScript objects, but it is a text format with its own syntax rules.

I can convert between JSON text and JavaScript objects.

### JSON.parse()

Converts JSON text into a JavaScript value.

```javascript
const text = '{"username":"Alice","level":5}';

const player = JSON.parse(text);

console.log(player.username);
```

### JSON.stringify()

Converts a JavaScript value into JSON text.

```javascript
const player = {
    username: "Alice",
    level: 5
};

const text = JSON.stringify(player);

console.log(text);
```

This is useful when exchanging information with a server or temporarily storing data.

## 15. Asynchronous JavaScript

Some operations take time to complete.

For example:

- Requesting information from a server.
- Loading data from an API.
- Reading files.
- Waiting for a database operation.

JavaScript uses asynchronous programming to avoid blocking the rest of the application while waiting for these operations.

### Promises

A Promise represents the eventual completion or failure of an asynchronous operation.

A promise can be:

- Pending.
- Fulfilled.
- Rejected.

I can handle a promise using `.then()` and `.catch()`:

```javascript
someAsyncOperation()
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

### async and await

`async` and `await` provide a more readable way to work with promises.

```javascript
async function loadPlayer() {
    const result = await someAsyncOperation();

    console.log(result);
}
```

An `async` function always returns a Promise.

I can use `try` and `catch` to handle errors:

```javascript
async function loadPlayer() {
    try {
        const result = await someAsyncOperation();

        console.log(result);
    } catch (error) {
        console.error("Something went wrong:", error);
    }
}
```

I should understand that `await` pauses the execution of the current async function until the promise settles. It does not freeze the entire browser.

## 16. Fetch and APIs

An API (Application Programming Interface) allows different software components to communicate.

For my Cursus application, JavaScript will eventually need to communicate with a backend server.

I can use the Fetch API to make HTTP requests.

For example:

```javascript
async function loadPlayers() {
    const response = await fetch("/api/players");

    if (!response.ok) {
        throw new Error("Could not load players");
    }

    const players = await response.json();

    console.log(players);
}
```

The server responds with JSON, which I can convert into JavaScript objects.

### HTTP Methods

Some common HTTP methods:

| Method | Purpose |
|---|---|
| `GET` | Retrieve information |
| `POST` | Create a resource or submit data |
| `PUT` | Replace a resource |
| `PATCH` | Partially update a resource |
| `DELETE` | Delete a resource |

For example, I could retrieve a player's information:

```javascript
fetch("/api/players/1");
```

Create a new project:

```javascript
fetch("/api/projects", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({
        name: "My First Project"
    })
});
```

I should remember that `fetch()` does not automatically reject its Promise when the server responds with an HTTP error such as 404 or 500. I need to check `response.ok`.

## 17. Browser Storage

The browser provides several ways to store information locally.

### localStorage

`localStorage` stores key-value pairs as strings and persists across browser sessions.

```javascript
localStorage.setItem("username", "Alice");

const username = localStorage.getItem("username");

localStorage.removeItem("username");
```

I can store an object by converting it into JSON:

```javascript
const player = {
    username: "Alice",
    level: 5
};

localStorage.setItem("player", JSON.stringify(player));
```

And retrieve it:

```javascript
const storedPlayer = localStorage.getItem("player");

const player = JSON.parse(storedPlayer);
```

### sessionStorage

`sessionStorage` works similarly but is generally limited to the lifetime of a browser tab's page session.

### Important Distinction

I should not confuse browser storage with a real database.

For Cursus:

- `localStorage` is useful for preferences and temporary local information.
- SQLite is useful for persistent application data on the server.
- The backend should be responsible for important player progress, achievements, and account information.

I should never trust a value stored in the browser when it affects permissions, achievements, or progression.

## 18. JavaScript Modules

As my application grows, putting everything in one JavaScript file will become difficult to maintain.

JavaScript modules allow me to split code into separate files.

For example:

```text
cursus/
    index.html
    css/
        style.css
    js/
        main.js
        player.js
        quests.js
        skills.js
        navigation.js
        api.js
```

I can export functions and import them into other files.

### Export

In `player.js`:

```javascript
export function createPlayer(username) {
    return {
        username,
        level: 1,
        experience: 0
    };
}
```

### Import

In `main.js`:

```javascript
import { createPlayer } from "./player.js";

const player = createPlayer("Alice");

console.log(player);
```

In my HTML, I need to load the main JavaScript file as a module:

```html
