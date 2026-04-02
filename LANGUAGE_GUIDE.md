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
While SP is written in C++, you can create native addons in any language that supports C linkage and can interface with SP's internal `Value` layout.

#### ⚡ Zig Example
Zig's memory-safe and low-level features make it an excellent choice for SP addons.

```zig
const std = @import("std");

// SP uses NaN-boxing. A Value is a 64-bit integer.
pub const Value = extern struct {
    bits: u64,

    pub fn isNumber(self: Value) bool {
        return (self.bits & 0x7FF0000000000000) != 0x7FF0000000000000;
    }

    pub fn asNumber(self: Value) f64 {
        return @bitCast(self.bits);
    }
};

// Layout of std::vector<Value> (GCC/Clang standard)
pub const StdVector = extern struct {
    start: [*]Value,
    finish: [*]Value,
    end_of_storage: [*]Value,

    pub fn len(self: *const StdVector) usize {
        return (@intFromPtr(self.finish) - @intFromPtr(self.start)) / @sizeOf(Value);
    }
};

export fn add_numbers(interp: ?*anyopaque, args: *const StdVector) u64 {
    _ = interp;
    if (args.len() < 2) return 0; // Return undefined bits or error
    
    const v1 = args.start[0];
    const v2 = args.start[1];
    
    if (v1.isNumber() and v2.isNumber()) {
        const res = v1.asNumber() + v2.asNumber();
        return @bitCast(res);
    }
    return 0;
}
```
**Compile:** `zig build-lib -dynamic addon.zig -femit-bin=libmy_addon.so`

#### 🦀 Rust Example
To use Rust, configure your project as a `cdylib` in your `Cargo.toml`.

```rust
#[repr(C)]
pub struct Value { bits: u64 }

impl Value {
    pub fn is_number(&self) -> bool {
        (self.bits & 0x7FF0000000000000) != 0x7FF0000000000000
    }
}

#[repr(C)]
pub struct StdVector {
    start: *const Value,
    finish: *const Value,
    end_of_storage: *const Value,
}

impl StdVector {
    pub fn len(&self) -> usize {
        (self.finish as usize - self.start as usize) / 8
    }
}

#[no_mangle]
pub unsafe extern "C" fn add_numbers(_i: *mut (), args: *const StdVector) -> u64 {
    let args = &*args;
    if args.len() < 2 { return 0; }
    
    let v1 = &*args.start;
    let v2 = &*args.start.add(1);
    
    if v1.is_number() && v2.is_number() {
        let res = f64::from_bits(v1.bits) + f64::from_bits(v2.bits);
        res.to_bits()
    } else {
        0
    }
}
```
**Cargo.toml:**
```toml
[lib]
crate-type = ["cdylib"]
name = "my_addon"
```

#### 🐹 Go Example
Go can create SP addons using `cgo`. Note that you must use `main` package.

```go
package main

/*
#include <stdint.h>
typedef struct { uint64_t bits; } Value;
typedef struct { Value* s; Value* f; Value* e; } StdVector;
*/
import "C"
import (
	"unsafe"
	"math"
)

//export add_numbers
func add_numbers(interp unsafe.Pointer, args *C.StdVector) C.uint64_t {
	count := (uintptr(unsafe.Pointer(args.f)) - uintptr(unsafe.Pointer(args.s))) / 8
	if count < 2 { return 0 }

	start := (*[1 << 30]C.Value)(unsafe.Pointer(args.s))[:count:count]
    v1, v2 := uint64(start[0].bits), uint64(start[1].bits)
    
    isNum := func(b uint64) bool { return (b & 0x7FF0000000000000) != 0x7FF0000000000000 }
    
    if isNum(v1) && isNum(v2) {
        res := math.Float64frombits(v1) + math.Float64frombits(v2)
        return C.uint64_t(math.Float64bits(res))
    }
	return 0
}

func main() {}
```
**Compile:** `go build -o libmy_addon.so -buildmode=c-shared main.go`

#### 🏺 Odin Example
```odin
package main

import "core:math"

Value :: struct { bits: u64 }
StdVector :: struct { start, finish, end: ^Value }

is_number :: proc(v: Value) -> bool {
    return (v.bits & 0x7FF0000000000000) != 0x7FF0000000000000
}

@(export)
add_numbers :: proc "c" (interp: rawptr, args: ^StdVector) -> u64 {
    len := (int(uintptr(args.finish)) - int(uintptr(args.start))) / size_of(Value)
    if len < 2 do return 0

    v1 := args.start[0]
    v2 := args.start[1]

    if is_number(v1) && is_number(v2) {
        res := transmute(f64)v1.bits + transmute(f64)v2.bits
        return transmute(u64)res
    }
    return 0
}
```
**Compile:** `odin build addon.odin -build-mode:shared -out:libmy_addon.so`

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

> [!TIP]
> Always ensure your shared object starts with the `lib` prefix (e.g., `libmath.so`) to be correctly discovered by the `use` statement in SP.


---

Happy Coding with **SP**! 
