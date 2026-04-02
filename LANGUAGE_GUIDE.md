# SP Language Guide

Welcome to the **SP Programming Language**! SP is a modern, strongly-typed, and expressive language designed for simplicity and performance. Whether you are a beginner or an experienced developer, this guide will help you get started.

---

## Table of Contents
1. [Getting Started](#getting-started)
2. [Variables & Mutability](#variables--mutability)
3. [Data Types](#data-types)
4. [Control Flow](#control-flow)
5. [Functions & Closures](#functions--closures)
6. [The Pipeline Operator (`|>`)](#the-pipeline-operator)
7. [Standard Library](#standard-library)
8. [Object-Oriented Programming (Classes)](#object-oriented-programming)
9. [IDE Support (VS Code)](#ide-support)

---

## Getting Started

To run an SP script, use the `sp` executable included in this repository:

```bash
./sp hello.sp
```

### Your First Script: Hello World
Create a file named `hello.sp` and add the following:

```sp
console.show("Hello, World!")
```

---

## IDE Support

For the best development experience, we recommend using the **SP Language** extension for **Visual Studio Code**.

### Features
- **Syntax Highlighting**: Beautiful coloring for the modern SP syntax.
- **IntelliSense**: Auto-completion for variables, functions, and object properties (including nested ones like `obj.a.b`).
- **Hover Information**: Instant documentation and type inference when hovering over identifiers.
- **Diagnostics**: Real-time error reporting for syntax and simple type mismatches.

### Installation
1.  Download the `sp-language-vscode-0.1.4.vsix` file from the repository.
2.  Install it via the VS Code extensions menu or the command line:
    ```bash
    code --install-extension sp-language-vscode-0.1.4.vsix
    ```


---

## Variables & Mutability

SP distinguishes between constants and mutable variables.

- **`set`**: Creates an **immutable** variable (a constant). Once set, it cannot be changed.
- **`var`**: Creates a **mutable** variable. Use this if you need to update the value later.

```sp
set name = "Alice"   // This cannot be changed
var age = 25         // This can be updated
age = 26             // Works!
```

---

## Data Types

SP supports several built-in types:

- **Numbers**: `10`, `3.14`
- **Strings**: `"Hello"`, `"Value is: {x}"` (Suppports interpolation using `{ }`)
- **Booleans**: `true`, `false`
- **BigInts**: `1000000000000000000n` (Suffix with `n`)
- **Arrays**: `[1, 2, 3]`
- **Objects**: `{name: "Bob", age: 30}`
- **Null & Undefined**: `null`, `undefined`

---

## Control Flow

### If / Else
`if` can be used as a statement or an expression.

```sp
set x = 10
if x > 5 {
    console.show("Big")
} else {
    console.show("Small")
}

// As an expression:
set status = if x > 5 { "Active" } else { "Inactive" }
```

### Match Expression (Pattern Matching)
A cleaner way to handle multiple conditions.

```sp
set choice = 2
set name = match choice {
    1: "One",
    2: "Two",
    default: "Unknown"
}
```

### Loops
- **While**: `while condition { ... }`
- **For-In**: `for item in collection { ... }`

```sp
var i = 0
while i < 5 {
    console.show(i)
    i = i + 1
}

for n in [10, 20, 30] {
    console.show("Number: {n}")
}
```

---

## Functions & Closures

### Defining Functions
Use the `define` keyword to create named functions.

```sp
define add = (a, b) => a + b

define greet = (name) => {
    console.show("Hello, {name}!")
}
```

### Closures
Functions can capture variables from their outer scope.

```sp
define makeAdder = (x) => {
    return (y) => x + y
}
set add5 = makeAdder(5)
console.show(add5(10)) // Outputs 15
```

---

## The Pipeline Operator

The pipe operator `|>` allows you to chain function calls in a readable left-to-right fashion.

```sp
define double = (x) => x * 2
define log = (x) => console.show("Result: {x}")

// Without pipes: log(double(10))
// With pipes:
10 |> double |> log
```

### Placeholders (`_`)
If a function takes multiple arguments, use `_` to represent the piped value.

```sp
define greet = (greeting, name) => console.show("{greeting}, {name}!")

"Alice" |> greet("Hello", _) // Outputs: Hello, Alice!
```

---

## Standard Library

### `console`
- `console.show(args...)`: Prints values to the screen.
- `console.read()`: Reads a line of text from the user.

### `fs` (File System)
- `fs.read(path)`: Returns file content as a string.
- `fs.write(path, content)`: Writes content to a file.
- `fs.append(path, content)`: Appends content to a file.
- `fs.delete(path)`: Deletes a file.
- `fs.info(path)`: Returns an object with file details.
- `fs.readJson(path)`: Reads a JSON file and parses it into an SP object.
- `fs.writeJson(path, content)`: Converts an SP object to JSON and writes it to a file.

### `math`
- Use `use math` to access math functions like `math.add(a, b)`.
- Global functions: `floor(n)`, `time()`.

### `Map`
SP provides a `Map` (also aliased as `HashMap`) for efficient key-value storage.

```sp
set myMap = Map()
myMap.set("key", "value")
console.show(myMap.get("key"))
```

### `Date`
Use the `Date` object to work with time.

```sp
set today = Date.now()
console.show("Current timestamp: {today}")
```

### `process`
- `process.run(cmd, args)`: Executes a command and waits for it to finish.
- `process.spawn(cmd, args)`: Starts a background process.

---

## Object-Oriented Programming

SP supports classes with modern features like access modifiers.

```sp
class Person {
    readonly name           // Read-only property
    private secret          // Private property
    var age                 // Mutable property

    define init = (name, secret, age) => {
        this.name = name
        this.secret = secret
        this.age = age
    }

    define sayHello = () => {
        console.show("Hi, I'm {this.name}")
    }
}

set bob = Person("Bob", "TopSecret", 30)
bob.sayHello()
```

### Abstract Classes
Use `abstract class` for base classes that shouldn't be instantiated directly.

---

Enjoy coding in **SP**! If you find any bugs, deal with it (by reporting to SwyftPain)!
