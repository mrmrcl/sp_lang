# 🌌 SP Language Guide

Welcome to the **SP Programming Language**! SP is a modern, strongly-typed, and expressive language designed for performance and simplicity. It features a self-contained compiler and virtual machine, accessible through a single, statically-linked standalone executable.

## 📖 Table of Contents
- [🚀 Getting Started](#-getting-started)
  - [1. Download & Prepare](#1-download--prepare)
  - [2. Run Your First Script](#2-run-your-first-script)
- [📘 IDE Support & Documentation](#-ide-support--documentation)
  - [Doc-Comments](#doc-comments)
- [🧩 Variables & Types](#-variables--types)
  - [Declarations](#declarations)
  - [Destructuring & Spread](#destructuring--spread)
  - [BigInts](#bigints)
- [🎮 Control Flow](#-control-flow)
  - [If / Else](#if--else)
  - [Match Expressions (Pattern Matching)](#match-expressions-pattern-matching)
  - [Loops](#loops)
- [🎯 Functions & Callbacks](#-functions--callbacks)
  - [Named Functions & Rest Params](#named-functions--rest-params)
  - [Implicit Returns](#implicit-returns)
  - [Callbacks (Higher-Order Functions)](#callbacks-higher-order-functions)
  - [Pipeline Operator (|>)](#pipeline-operator--)
- [📦 Modules & Imports](#-modules--imports)
  - [Named Imports & Aliasing](#named-imports--aliasing)
- [📦 Standard Library](#-standard-library)
  - [📁 fs (File System) & JSON](#-fs-file-system--json)
  - [💻 console & Benchmarking](#-console--benchmarking)
  - [🌐 Global Functions](#-global-functions)
  - [⚙️ process](#-process)
  - [🗺️ Map (HashMap)](#-map-hashmap)
  - [📅 Date](#-date)
- [🏛️ Object-Oriented Programming](#-object-oriented-programming)
  - [Access Modifiers](#access-modifiers)
- [🧙‍♂️ Advanced: Native Addons (C++ Integration)](#-advanced-native-addons-c-integration)
  - [1. File Placement & Naming](#1-file-placement--naming)
  - [2. Write the C++ Code (my_func.cpp)](#2-write-the-c-code-my_funccpp)
  - [3. Compile to Shared Object](#3-compile-to-shared-object)
  - [4. Use in SP](#4-use-in-sp)
  - [🧙‍♂️ Cross-Language Addons](#-cross-language-addons)
  - [⚡ Optimization: Minimal C Header](#-optimization-minimal-c-header)
  - [🛠️ Compiling & Linking Addons](#-compiling--linking-addons)

---

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

// Nested Destructuring
set [a, [b, c]] = [1, [2, 3]]
set personMap = { info: { id: 101 } }
set { info: { id } } = personMap
console.show(id) // 101
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

### BigInts
For working with arbitrary-precision integers, use the `n` suffix.
```sp
set big = 1000000000000000000n
set small = 10n
console.show(big + small) // 1000000000000000010n
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

### Implicit Returns
Functions in SP return the value of the last expression in their block if no `return` is used.
```sp
define multiply = (a, b) => a * b // Implicitly returns a * b

define check = (x) => {
    if x > 10 { "Big" } else { "Small" } // Implicitly returns branch value
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

### ⚙️ `process`
Execute system commands and manage external processes.

| Method | Description |
| :--- | :--- |
| `process.run(cmd, args)` | Executes a command, waits for it to finish, and returns the exit code. |
| `process.spawn(cmd, args)` | Starts a command as a background process. |

```sp
// Example: List files in current directory
process.run("ls", ["-la"])
```

### 🗺️ `Map` (HashMap)
SP provides a `Map` structure for high-performance key-value storage.

```sp
set scores = Map()
scores.set("Alice", 100)
scores.set("Bob", 85)

console.show(scores.get("Alice")) // 100
console.show(scores.has("Charlie")) // false

// Size property
console.show("Count: {scores.size}")

// Advanced methods
scores.delete("Bob")
scores.keys()   // ["Alice"]
scores.values() // [100]
scores.clear()  // Empty the map

// forEach callback (key, value)
scores.set("X", 1).set("Y", 2).forEach((k, v) => {
    console.show("{k} = {v}")
})
```

Mention: `HashMap` is a valid alias for `Map`.

### 📅 `Date`
Interact with system time and timestamps.

```sp
set now = Date.now()
console.show("Current timestamp: {now}")

// Date Properties
console.show("Year: {now.year}")
console.show("Month: {now.month}")
console.show("Day: {now.day}")
console.show("Time: {now.hour}:{now.minute}:{now.second}")

set info = fs.info("hello.sp")
console.show("File created at: {info.createdAt}")
```

---

## 🏛️ Object-Oriented Programming

```sp
class Robot {
    readonly id
    var power = 100

    define init = (id) => this.id = id
    define charge = () => console.show("Robot {this.id} charged.")
}
```

### Access Modifiers
SP supports several property and class modifiers for better encapsulation:
- **`readonly`**: Value can only be set during initialization.
- **`private`**: Only accessible within the class itself.
- **`abstract class`**: A base class that cannot be instantiated directly.

```sp
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
All arguments passed from SP are accessible in the `std::vector<Value>& args` parameter.

```cpp
#include "types.h"
#include <vector>
#include <stdexcept>

extern "C" {
    uint64_t add_numbers(Interpreter& interp, const std::vector<Value>& args) {
        if (args.size() < 2) {
            throw std::runtime_error("add_numbers expects 2 arguments");
        }
        if (!args[0].isNumber() || !args[1].isNumber()) {
            throw std::runtime_error("add_numbers expects numeric arguments");
        }
        
        double result = args[0].asNumber() + args[1].asNumber();
        return Value(result).bits; // Always return Value.bits
    }
}
```

### 3. Compile to Shared Object
```bash
g++ -O3 -shared -fPIC my_func.cpp -Iinclude -o libmy_addon.so
```

### 4. Use in SP
```sp
// SP will search for libmy_addon.so in ./ or ./modules/
use { add_numbers } from "my_addon"

console.show(add_numbers(10, 25)) // Outputs: 35
```

### 🧙‍♂️ Cross-Language Addons
While SP is written in C++, you can create native addons in any language that supports C linkage. We maintain a dedicated guide with examples and compilation steps for a wide range of modern languages:

👉 **[Native Addons Guide (Zig, Rust, Go, Nim, Crystal, Swift, D, etc.)](./NATIVE_ADDONS_GUIDE.md)**

### ⚡ Optimization: Minimal C Header
To simplify addon development in non-C++ languages, you can use our minimal C99-compatible header: [**`sp_addon.h`**](./include/sp_addon.h).

This header defines the `sp_value` and `sp_args` (vector layout) types, along with helper macros. This means you do not need to parse the full C++ `types.h` and can instead work with pure C structures.

```c
#include "sp_addon.h"

// Example usage in C
uint64_t add_numbers(void* i, const sp_args* args) {
    if (SP_ARGS_LEN(args) < 2) return 0;
    
    sp_value v1 = SP_GET_ARG(args, 0);
    sp_value v2 = SP_GET_ARG(args, 1);
    
    if (SP_IS_NUMBER(v1) && SP_IS_NUMBER(v2)) {
        double res = SP_AS_NUMBER(v1) + SP_AS_NUMBER(v2);
        return SP_MAKE_NUMBER(res).bits;
    }
    return 0;
}
```

### 🛠️ Compiling & Linking Addons
When compiling your addon, there are a few critical requirements:

1.  **Shared Library**: The output MUST be a shared object (`.so`).
2.  **Position Independent Code**: You must use `-fPIC` (C/C++) or equivalent options.
3.  **No Linking Required**: Because the SP interpreter is self-contained, your addon **does not need to link against the SP binary** at compile-time. Symbols are resolved dynamically when the addon is loaded.

#### Unified Compilation Commands

| Language | Command |
| :--- | :--- |
| **C / C++** | `g++ -O3 -shared -fPIC addon.cpp -Iinclude -o libmy_addon.so` |
| **Zig** | `zig build-lib -dynamic addon.zig -femit-bin=libmy_addon.so` |
| **Rust** | `cargo build --release` (with `cdylib` in `Cargo.toml`) |
| **Go** | `go build -o libmy_addon.so -buildmode=c-shared main.go` |
| **Odin** | `odin build addon.odin -build-mode:shared -out:libmy_addon.so` |
| **Nim** | `nim c --app:lib -d:release addon.nim` |
| **Crystal** | `crystal build --shared addon.cr` |
| **Swift** | `swiftc -emit-library -o libmy_addon.so addon.swift` |
| **D** | `ldc2 -shared addon.d -of=libmy_addon.so` |
| **FreePascal** | `fpc -Mdelphi -Tlinux -Pauto -Wl-shared addon.pas` |
| **Python** | `cython addon.pyx && gcc -shared -o libmy_addon.so addon.c $(python3-config --includes --ldflags) -fPIC` |
| **C#** | `dotnet publish -c Release -r linux-x64 /p:NativeLib=Shared` |
| **Java** | `native-image --shared -H:Name=libmy_addon` |
| **Julia** | `using PackageCompiler; create_library(...)` |
| **JavaScript** | `gcc -shared -fPIC addon.c -lquickjs -o libmy_addon.so` |
| **Kotlin** | `kotlinc-native addon.kt -produce library -o libmy_addon` |
| **Haskell** | `ghc -shared -dynamic -fPIC addon.hs -o libmy_addon.so` |
| **Fortran** | `gfortran -shared -fPIC addon.f90 -o libmy_addon.so` |
| **Assembly** | `nasm -f elf64 addon.asm -o addon.o && gcc -shared -fPIC addon.o -o libmy_addon.so` |
| **COBOL** | `cobc -z -O3 -shared addon.cob -o libmy_addon.so` |
| **LOLCODE** | `gcc -shared -fPIC addon.c -llolcode -o libmy_addon.so` |
| **Brainfuck** | `gcc -shared -fPIC bf_wrapper.c -o libmy_addon.so` |
| **Common Lisp** | `ecl -build-node shared-library -o libmy_addon.so addon.lisp` |
| **Bash** | `gcc -O3 -shared -fPIC addon.c -o libmy_addon.so` |

> [!TIP]
> Always ensure your shared object starts with the `lib` prefix (e.g., `libmath.so`) to be correctly discovered by the `use` statement in SP.


---

Happy Coding with **SP**! 
