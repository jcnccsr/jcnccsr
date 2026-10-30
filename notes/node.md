---
layout: page
title: Node.js Basics
description: >
    Notes I keep for myself while learning Node.js and running JavaScript outside the browser.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What is Node.js?

JavaScript is a programming language that was originally created to run inside web browsers.

Node.js allows us to run JavaScript outside the browser, directly on our computer or server.

This means I can use JavaScript to:

- Build web servers.
- Read and write files.
- Work with databases.
- Create command-line tools.
- Run background programs.
- Build the backend of a web application.

Node.js uses the V8 JavaScript engine, the same engine used by Google Chrome.

An important distinction:

- **JavaScript** is the language.
- **Node.js** is a runtime environment for executing JavaScript.
- **npm** is a package manager commonly used with Node.js.

## Installing and checking Node.js

On my Mac, I can install Node.js using Homebrew:

```bash
brew install node
```

On Debian-based Linux systems, I can install it through the package manager:

```bash
sudo apt update
sudo apt install nodejs npm
```

I can check whether everything is installed:

```bash
node --version
npm --version
```

The first command displays the Node.js version, and the second displays the npm version.

## Running JavaScript

### Running a JavaScript file

I can create a file called `main.js`:

```javascript
console.log("Hello, Node.js!");
```

Then execute it from the terminal:

```bash
node main.js
```

Node.js executes the JavaScript file and displays the output.

Unlike browser JavaScript, I don't need an HTML document to run the program.

### The Node.js REPL

Node.js also provides an interactive environment called the REPL.

REPL stands for:

- Read
- Evaluate
- Print
- Loop

I can start it by typing:

```bash
node
```

Then enter JavaScript directly:

```javascript
> 5 + 5
10

> let name = "Peasant"
undefined

> console.log(name)
Peasant
```

I can leave the REPL by typing:

```javascript
.exit
```

This is useful for testing small pieces of JavaScript without creating a file.

## Node.js versus browser JavaScript

Both environments execute JavaScript, but they provide different features.

| Browser | Node.js |
|---|---|
| Runs JavaScript in a browser | Runs JavaScript outside a browser |
| Has access to the DOM | No browser DOM by default |
| Can interact with HTML elements | Can interact with the filesystem |
| Uses browser APIs | Uses Node.js APIs |
| Commonly handles frontend logic | Commonly handles backend logic |

For example, this works in a browser:

```javascript
document.querySelector("h1");
```

But it doesn't work in a normal Node.js program because Node.js doesn't have the browser's `document` object.

Node.js provides its own APIs instead.

## The console

I can use `console` to display information while my program runs.

```javascript
console.log("Normal message");
console.error("An error occurred");
console.warn("Be careful!");
```

I can also display multiple values:

```javascript
let name = "Peasant";
let age = 30;

console.log(name, age);
```

And I can use template literals:

```javascript
console.log(`My name is ${name} and I am ${age} years old.`);
```

## The process object

Node.js provides a global object called `process`.

It contains information about the currently running Node.js program.

For example:

```javascript
console.log(process.version);
console.log(process.platform);
console.log(process.cwd());
```

These display:

- The Node.js version.
- The operating system platform.
- The current working directory.

### Command-line arguments

I can also retrieve arguments passed to my program through `process.argv`.

For example:

```javascript
console.log(process.argv);
```

If I execute:

```bash
node main.js hello world
```

The array contains information about the Node.js executable, the script path, and the arguments I supplied.

I can retrieve my own arguments like this:

```javascript
let arguments = process.argv.slice(2);

console.log(arguments);
```

Now the output will contain:

```text
[ 'hello', 'world' ]
```

This is useful when creating command-line applications.

## Modules

As my programs become larger, I don't want to put everything into one JavaScript file.

I can split my code into separate files and import functionality from one file into another.

Node.js supports the modern JavaScript module system, called ECMAScript Modules (ES Modules).

### Exporting a function

Let's create a file called `math.js`:

```javascript
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}
```

I can export functions, variables, classes, and other values.

### Importing a function

Now I create `main.js`:

```javascript
import { add, subtract } from "./math.js";

console.log(add(10, 5));
console.log(subtract(10, 5));
```

To use this module syntax in `.js` files, I can add a `package.json` file containing:

```json
{
    "type": "module"
}
```

This tells Node.js to interpret `.js` files in this project as ES modules.

Another option is to use the `.mjs` file extension.

## Built-in modules

Node.js comes with modules that provide useful functionality without installing additional packages.

Some examples:

| Module | Purpose |
|---|---|
| `node:fs` | Work with files and directories |
| `node:path` | Work with file paths |
| `node:http` | Create HTTP servers |
| `node:os` | Retrieve operating system information |
| `node:crypto` | Cryptographic functionality |

For example, I can use the filesystem module to read a file.

First, create a file called `message.txt`:

```text
Hello from a text file!
```

Then, in `main.js`:

```javascript
import { readFile } from "node:fs/promises";

const message = await readFile("message.txt", "utf8");

console.log(message);
```

Here, `readFile()` reads the file and returns its contents.

The `await` keyword allows me to wait for the asynchronous operation to finish before continuing.

I'll need to learn more about asynchronous JavaScript later.

## npm — Node Package Manager

Node.js is accompanied by npm, which allows me to install and manage external JavaScript packages.

A package is reusable code that someone has created and made available for other developers.

Instead of writing everything myself, I can use existing packages.

### Creating a project

I can create a directory for my project:

```bash
mkdir my-project
cd my-project
```

Then initialize it:

```bash
npm init -y
```

This creates a `package.json` file.

### What is package.json?

The `package.json` file contains information about my project.

For example:

```json
{
    "name": "my-project",
    "version": "1.0.0",
    "type": "module",
    "description": "My first Node.js project",
    "scripts": {
        "start": "node main.js"
    }
}
```

Some important fields:

- `name`: Project name.
- `version`: Project version.
- `type`: Module system used by `.js` files.
- `description`: A short project description.
- `scripts`: Commands I can execute through npm.

### Installing a package

For example:

```bash
npm install package-name
```

This installs a package and its dependencies.

npm creates a `node_modules` directory where the installed packages are stored.

It also updates `package.json` and creates or updates `package-lock.json`, which records the resolved dependency versions.

I should generally avoid committing `node_modules` to Git. It can be recreated from the package information.

### Running scripts

If my `package.json` contains:

```json
"scripts": {
    "start": "node main.js"
}
```

I can run the program with:

```bash
npm start
```

Instead of typing `node main.js` every time.

## A very simple HTTP server

One of the interesting things about Node.js is that I can create a web server using its built-in HTTP module.

Create `server.js`:

```javascript
import { createServer } from "node:http";

const server = createServer((request, response) => {
    response.writeHead(200, {
        "Content-Type": "text/plain"
    });

    response.end("Hello from my Node.js server!");
});

server.listen(3000, () => {
    console.log("Server running at http://localhost:3000");
});
```

Run it:

```bash
node server.js
```

Then open this address in a browser:

```text
http://localhost:3000
```

I should see:

```text
Hello from my Node.js server!
```

A few things to understand:

- `createServer()` creates an HTTP server.
- `request` contains information about the incoming request.
- `response` is what I use to send something back.
- `writeHead()` sets the HTTP status and response headers.
- `end()` sends the response and finishes it.
- `listen()` tells the server to listen for connections on a port.

This is a very small example, but it demonstrates how JavaScript can power the backend of a web application.

## Useful Node.js and npm commands

| Command | Purpose |
|---|---|
| `node --version` | Display Node.js version |
| `npm --version` | Display npm version |
| `node main.js` | Run a JavaScript file |
| `node` | Open the Node.js REPL |
| `npm init -y` | Initialize a project |
| `npm install` | Install project dependencies |
| `npm install package-name` | Install a package |
| `npm uninstall package-name` | Remove a package |
| `npm start` | Run the start script |
| `npm run` | List available scripts |

## What I should learn next

These are some subjects I will eventually need to understand to become comfortable with Node.js:

1. JavaScript fundamentals.
2. Objects, arrays, and functions.
3. ES Modules and code organization.
4. Asynchronous JavaScript.
5. Callbacks, Promises, and `async` / `await`.
6. Filesystem operations.
7. HTTP and web servers.
8. REST APIs.
9. Working with databases.
10. Error handling and testing.

I don't need to learn everything at once. I should understand the fundamentals first and build small programs as I go.

## My current understanding

Node.js allows me to use JavaScript outside the browser.

I can execute scripts, interact with the filesystem, import modules, install packages, and create web servers.

The most important thing for now is understanding that Node.js is not another programming language. It is an environment that gives JavaScript access to capabilities beyond the browser.