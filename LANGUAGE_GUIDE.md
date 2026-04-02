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

## 📘 IDE Support

For the best experience, use our **[Visual Studio Code extension](./sp-language-vscode-0.1.4.vsix)**.
- **Features**: Syntax highlighting, Diagnostics, Hover Info, and IntelliSense.
- **Install**: `code --install-extension sp-language-vscode-0.1.4.vsix`

---

## 🧩 Variables & Types

### Declarations
- **`set`**: Immutable (constant).
- **`var`**: Mutable (can be reassigned).

```sp
set pi = 3.14159          // Constant
var score = 100           // Variable
score = 105               // Re-assignment
```

### Data Types
- **Numbers & BigInts**: `42`, `3.14`, `1000000000n`.
- **Strings**: Supports interpolation with `{ }`.
    ```sp
    set user = "Alice"
    console.show("Welcome, {user}!") 
    ```
- **Arrays**: `[1, 2, 3]`. Supports `.length`.
- **Objects**: `{key: "value"}`. Access properties with dot `.`.
- **Map / HashMap**: Built-in for efficient key-value storage.
    ```sp
    set store = Map()
    store.set("apples", 10)
    console.show(store.get("apples")) // 10
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
```sp
for name in ["Alice", "Bob"] {
    console.show("Hi, {name}")
}
```

---

## 🎯 Functions & Closures

### Named Functions
```sp
define add = (a, b) => a + b
```

### Pipeline Operator (`|>`)
Chain function calls from left to right. Use `_` as a placeholder for the piped value.
```sp
define double = (n) => n * 2
10 |> double |> console.show(_) // Outputs: 20
```

---

## 📦 Standard Library

### 📁 `fs` (File System)
The `fs` module handles all file operations.

| Method | Description |
| :--- | :--- |
| `fs.read(path)` | Returns file content as a string or `null`. |
| `fs.write(path, data)` | Overwrites a file with the provided data. |
| `fs.append(path, data)` | Appends data to an existing file. |
| `fs.delete(path)` | Deletes a file. |
| `fs.readJson(path)` | Reads and parses a JSON file into an SP object. |
| `fs.writeJson(path, obj)` | Serializes an SP object to JSON and writes to disk. |

#### `fs.info(path)`
Returns a detailed object for a file:
```sp
set info = fs.info("data.txt")
console.show("Size:", info.size)        // Bytes
console.show("Extension:", info.ext)    // e.g. ".txt"
console.show("Modified:", info.modifiedAt) // Date object
```
**Return Object Details**:
- `path` (string): Absolute path.
- `dirname` (string): Parent directory.
- `name` (string): Filename with extension.
- `ext` (string): Extension (includes `.`).
- `size` (number): File size in bytes.
- `exists` (boolean): Whether the file exists.
- `modifiedAt` (Date): Last modification time.
- `createdAt` (Date): Creation time.

### 💻 `console`
| Method | Description |
| :--- | :--- |
| `console.show(...)` | Prints values to stdout with a newline. |
| `console.warn(...)` | Prints values to stderr in yellow. |
| `console.read()` | Reads a line from the user's input. |
| `console.args()` | Returns an **Array** of CLI arguments passed to the script. |

### 🛠️ `process`
| Method | Description |
| :--- | :--- |
| `process.run(cmd, args)` | Sync: Runs command and returns exit code. |
| `process.spawn(cmd, args)`| Async: Starts a background process. |

### 🗺️ `Map` / `HashMap`
| Method | Description |
| :--- | :--- |
| `set(key, val)` | Stores a value. Returns Map (for chaining). |
| `get(key)` | Retrieves a value or `null`. |
| `has(key)` | Returns `true` if the key exists. |
| `delete(key)` | Removes an entry. |
| `clear()` | Removes all entries. |
| `keys()` | Returns an **Array** of all keys. |
| `values()` | Returns an **Array** of all values. |
| `forEach(fn)` | Calls `fn(key, value)` for each entry. |

### 🕒 `Date`
Created with `Date.now()`. Properties:
- `year`, `month` (1-12), `day`.
- `hour`, `minute`, `second`.
- `toString()`: Formats as "YYYY-MM-DD HH:MM:S".

---

## 🏛️ Object-Oriented Programming

Define templates for reusable objects using `class`.

```sp
class Robot {
    readonly id
    var power = 100

    define init = (id) => {
        this.id = id
    }

    define charge = () => {
        this.power = 100
        console.show("Robot {this.id} charged.")
    }
}

set bot = Robot("SP-1")
bot.charge()
```

---

Happy Coding with **SP**! 
