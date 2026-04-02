# SP Programming Language

A modern, strongly-typed, and expressive language designed for performance and simplicity. SP features a self-contained compiler and virtual machine, accessible through a single standalone executable.

---

## 🚀 Quick Start

### 1. Build the Interpreter
The SP interpreter is designed to be statically linked and portable. Simply run:
```bash
make
```

### 2. Run Your First Script
Create a file named `hello.sp`:
```sp
console.show("Hello, SP World!")
```
Run it with the interpreter:
```bash
./sp_interpreter hello.sp
```

---

## 📘 Documentation

For a comprehensive guide on the language syntax, built-in modules, and advanced features like the **Pipeline Operator** and **Classes**, check out our **[Language Guide](file:///home/swyft/Documents/term/sp_language/LANGUAGE_GUIDE.md)**.

## ✨ Features at a Glance

- **Modern Syntax**: Expressive and clean logic with `set` (immutable) and `var` (mutable).
- **First-Class Functions**: Powerful closures and arrow functions.
- **Rich Built-in Types**: Extensive methods for Strings (`trim`, `split`, etc.), Arrays (`map`, `filter`, `push`, etc.), Objects, and Numbers.
- **Pipeline Operator (`|>`)**: Readable, left-to-right function chaining with placeholder support.
- **Pattern Matching**: Robust `match` expressions for complex condition handling.
- **Built-in Systems**: Direct access to `console`, `fs` (File System), and `process` utilities.
- **Object-Oriented**: Support for classes with `abstract`, `private`, and `readonly` modifiers.

---

Happy Coding with **SP**!