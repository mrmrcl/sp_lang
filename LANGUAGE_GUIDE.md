# SP Language Guide

Welcome to the **SP Programming Language**! SP is a modern and expressive language designed for simplicity and performance. Whether you are a beginner or an experienced developer, this guide will help you get started.

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

---

## Getting Started

To run an SP script, use the `sp` executable:

```bash
./sp hello.sp
```

### Your First Script: Hello World
Create a file named `hello.sp` and add the following:

```sp
console.show("Hello, World!")
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

## Type Methods

SP provides built-in methods for core data types to make manipulation easy and expressive.

### 🧵 String Methods
- `s.trim()`: Removes whitespace from both ends.
- `s.toLowerCase()` / `s.toUpperCase()`: Converts casing.
- `s.contains(sub)` / `s.includes(sub)`: Checks for a substring.
- `s.startsWith(sub)` / `s.endsWith(sub)`: Prefix/Suffix checks.
- `s.indexOf(sub)`: Returns the first index of `sub`, or `-1`.
- `s.split(sep)`: Splits the string into an array.
- `s.replace(from, to)`: Replaces first occurrence.
- `s.substring(start, end)`: Extracts a portion of the string.

### 📦 Array Methods
- `arr.push(val)` / `arr.pop()`: Add/remove from end.
- `arr.unshift(val)` / `arr.shift()`: Add/remove from beginning.
- `arr.length`: Returns the number of elements.
- `arr.join(sep)`: Joins elements into a string.
- `arr.reverse()`: Reverses the array **in-place**.
- `arr.slice(start, end)`: Returns a shallow copy of a portion.
- `arr.contains(val)` / `arr.includes(val)`: Checks for value.
- `arr.indexOf(val)`: Returns index of value.

#### ⚡ Functional Methods
- `arr.map(callback)`: Returns a new array with transformed items.
- `arr.filter(callback)`: Returns a new array with items that pass a test.
- `arr.forEach(callback)`: Executes a function for each element.

### 🗝️ Object Methods
- `obj.keys()`: Returns an array of property names.
- `obj.values()`: Returns an array of property values.
- `obj.has(key)`: Checks if a property exists.

### 🔢 Number Methods
- `n.toFixed(digits)`: Returns a string with fixed decimal points.
- `n.toString()`: Converts the number to a string.

#### 🔗 Method Chaining Example
You can chain methods together for concise and expressive code:
```sp
set result = "  Hello, world!  ".trim().toUpperCase().replace("WORLD", "SP")
console.show(result) // Outputs: HELLO, SP!
```

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

### Implicit Returns
Functions in SP return the value of the last expression in their block if no `return` is used.
```sp
define multiply = (a, b) => a * b // Implicitly returns a * b
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

### 🌐 Global Functions
These functions are available globally in every script without needing an import.

#### `time()`
Returns the current unix timestamp in **milliseconds**. This is useful for benchmarking or calculating durations.
```sp
set start = time()
// ... do some work ...
set end = time()
console.show("Took: {end - start}ms")
```

#### `floor(n)`
Rounds a number **down** to the nearest integer.
```sp
console.show(floor(10.7)) // Outputs: 10
console.show(floor(5.2))  // Outputs: 5
```

### `Map`
SP provides a `Map` (also aliased as `HashMap`) for efficient key-value storage.

```sp
set myMap = Map()
myMap.set("key", "value")
console.show(myMap.get("key"))

// Advanced: size, delete, clear, forEach
myMap.size // 1
myMap.delete("key")
myMap.clear()
myMap.forEach((k, v) => console.show(k, v))

// Note: HashMap is an alias for Map.
```

### `Date`
Use the `Date` object to work with time.

```sp
set today = Date.now()
console.show("Current timestamp: {today}")

// Properties: year, month, day, hour, minute, second
console.show(today.year)
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
Use `abstract class` for base classes that shouldn't be instantiated directly. Properties can also be marked as `readonly` or `private`.

---

Enjoy coding in **SP**! If you find any bugs, deal with it (by reporting to SwyftPain)!
