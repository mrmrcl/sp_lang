# 🌌 SP Language Guide

Welcome to the **SP Programming Language**! SP is a modern, strongly-typed, and expressive language designed for performance and simplicity. It features a self-contained compiler and virtual machine, accessible through a single, statically-linked standalone executable.

---

## 🚀 Getting Started

### 1. Download & Prepare
Download the `sp` executable from the repository and ensure it has execution permissions:
```bash
chmod +x sp
```

### 2. Run Your First Script
Create `hello.sp` and run it:
```sp
console.show("Hello, SP!")
```
```bash
./sp hello.sp
```

---

## 📘 IDE Support & Documentation

For the best experience, use our **[Visual Studio Code extension](./sp-language-vscode-0.1.4.vsix)**.

### Doc-Comments
SP supports JSDoc-style comments that provide instant IntelliSense and Type Inference in VS Code.
```sp
/**
 * Adds two numbers.
 * @param {number} a
 * @param {number} b
 * @return {number} The sum
 */
define add = (a, b) => a + b
```
*Hovering over `add` in VS Code will now show this documentation.*

---

## 🧩 Variables & Types

### Declarations
- **`set`**: Immutable (constant).
- **`var`**: Mutable (can be reassigned).

### Destructuring & Spread
SP supports modern destructuring for objects and arrays.

**Array Destructuring**:
```sp
set [first, second, ...rest] = [10, 20, 30, 40]
console.show(first) // 10
console.show(rest)  // [30, 40]
```

**Object Destructuring**:
```sp
set person = { name: "Alice", age: 30 }
set { name, age: years } = person
console.show(name)  // "Alice"
console.show(years) // 30
```

**Spread Operator (`...`)**:
```sp
set arr1 = [1, 2]
set arr2 = [0, ...arr1, 3] // [0, 1, 2, 3]

set obj1 = { a: 1 }
set obj2 = { ...obj1, b: 2 } // { a: 1, b: 2 }
```

---

## 🎮 Control Flow

### If / Else
`if` expressions return the value of the successful branch.
```sp
set status = if score > 100 { "Pro" } else { "Beginner" }
```

### Match Expressions (Pattern Matching)
A robust way to branch based on values.
```sp
set result = match score {
    100: "Perfect!",
    90: "Great!",
    default: "Keep trying"
}
```

### Loops
- **`while condition { ... }`**: Standard loop.
- **`for item in collection { ... }`**: Iterates over arrays.

---

## 🎯 Functions & Callbacks

### Named Functions & Rest Params
```sp
define sumAll = (...args) => {
    var total = 0
    for n in args { total = total + n }
    return total
}
```

### Callbacks (Higher-Order Functions)
Functions are first-class citizens. You can pass them as arguments to other functions like `arr.forEach`.
```sp
define processList = (list, callback) => {
    for item in list { callback(item) }
}

processList([1, 2, 3], (x) => console.show("Processing: {x}"))
```

### Pipeline Operator (`|>`)
Chain function calls from left to right. Use `_` as a placeholder.
```sp
10 |> ((n) => n * 2) |> console.show(_) // 20
```

---

## 📦 Modules & Imports

### Named Imports & Aliasing
```sp
// Standard Import
use math

// Named Import with Aliasing
use { add as plus, mul } from math

// Module Aliasing
use math as m
console.show(m.add(1, 2))
```

---

## 📦 Standard Library

### 📁 `fs` (File System) & JSON
| Method | Description |
| :--- | :--- |
| `fs.read(path)` | Returns file content as a string or `null`. |
| `fs.write(path, data)` | Overwrites a file. |
| `fs.readJson(path)` | Reads and parses JSON into an SP object. |
| `fs.writeJson(path, obj)` | Serializes and writes JSON. |

#### `fs.info(path)` (Exhaustive)
Returns a detailed object containing all file metadata:
- `path` (string): The absolute path to the file.
- `dirname` (string): Path to the parent directory.
- `name` (string): The filename including extension.
- `ext` (string): The file extension (e.g. `.sp`).
- `size` (number): File size in bytes.
- `length` (number): An alias for `size`.
- `exists` (boolean): `true` if the file/path exists.
- `modifiedAt` (Date): A **Date** object for the last modification time.
- `createdAt` (Date): A **Date** object for the file creation time.

### 💻 `console` & Benchmarking
| Method | Description |
| :--- | :--- |
| `console.args()` | Returns an **Array** of CLI arguments. |
| `console.warn(...)` | Prints a yellow warning to stderr. |
| `console.read()` | Reads a single line from standard input. |
| `time()` | Returns current timestamp in **milliseconds**. |
| `floor(n)` | Returns the floor of a number. |

---

## 🏛️ Object-Oriented Programming

```sp
class Robot {
    readonly id
    var power = 100

    define init = (id) => this.id = id
    define charge = () => console.show("Robot {this.id} charged.")
}

set bot = Robot("SP-1")
bot.charge()
```

---

## 🧙‍♂️ Advanced: Native Addons (C++ Integration)

SP allows you to load native C++ shared objects (`.so`) as modules.

### 1. File Placement & Naming
For a module called `"my_addon"`, SP searches for a file named **`libmy_addon.so`** in the following locations (in order):
1.  **`./`** (The current working directory)
2.  **`./modules/`** (A subfolder in your project)
3.  **System Paths** (Standard library locations)

### 2. Write the C++ Code (`my_func.cpp`)
To compile against the SP API, you must include **`types.h`**. These and other required headers are provided in the **`include/`** directory of this repository for development.

```cpp
#include "types.h"
#include <vector>

extern "C" {
    uint64_t my_native_func(Interpreter& interp, const std::vector<Value>& args) {
        return Value(42.0).bits; // Always return Value.bits
    }
}
```

### 3. Compile to Shared Object
Ensure you include the **`include/`** directory in your compiler's search path using **`-Iinclude`**:
```bash
g++ -O3 -shared -fPIC my_func.cpp -Iinclude -o libmy_addon.so
```


### 4. Use in SP
```sp
// SP will search for libmy_addon.so in ./ or ./modules/
use { my_native_func } from "my_addon"
console.show(my_native_func()) // 42
```

---

Happy Coding with **SP**! 
