# 🌌 SP Programming Language

A modern, strongly-typed, and expressive language designed for performance and simplicity. SP features a self-contained compiler and virtual machine, accessible through a single, statically-linked standalone executable.

---

## 🚀 Quick Start

### 1. Download the Interpreter
Download the `sp` executable from this repository. Ensure it has execution permissions:
```bash
chmod +x sp
```

### 2. Install the VS Code Extension (Recommended)
Enhance your development experience with the official SP extension for VS Code:
1.  Download the `sp-language-vscode-0.1.4.vsix` file from this repository.
2.  Install it via terminal:
    ```bash
    code --install-extension sp-language-vscode-0.1.4.vsix
    ```
    *Or, in VS Code, go to the Extensions view, click `...` (top-right), and select `Install from VSIX...`.*

### 3. Run Your First Script
Create a file named `hello.sp`:
```sp
console.show("Hello, SP World!")
```
Run it with the interpreter:
```bash
./sp hello.sp
```

---

## 📘 Documentation

For a comprehensive guide on the language syntax, built-in modules, and advanced features like the **Pipeline Operator** and **Classes**, check out our **[Language Guide](./LANGUAGE_GUIDE.md)** on the repository.

## ✨ Features at a Glance

- **Standalone Static Binary**: Includes the compiler and VM in one portable 3MB file.
- **IDE Support**: Advanced IntelliSense, syntax highlighting, and hover information via the included VS Code extension.
- **Embedded Standard Library**: Modules like `Math`, `Date`, `fs`, `Map`, and `process` are all pre-packaged within the binary.
- **Modern Syntax**: Expressive and clean logic with `set` (immutable) and `var` (mutable).
- **First-Class Functions**: Powerful closures and arrow functions.
- **Pipeline Operator (`|>`)**: Readable, left-to-right function chaining with placeholder support.
- **Pattern Matching**: Robust `match` expressions for complex condition handling.
- **Object-Oriented**: Support for classes with `abstract`, `private`, and `readonly` modifiers.

---

Happy Coding with **SP**!
