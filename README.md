# 🌌 SP Programming Language

A modern, strongly-typed, and expressive language designed for performance and simplicity. SP features a self-contained compiler and virtual machine, accessible through a single, statically-linked standalone executable.

---

## 🚀 Quick Start

### 1. Download the Interpreter
Download the `sp` executable from this repository. Ensure it has execution permissions:
```bash
chmod +x sp
```

### 2. Install the VS Code Extension
For the best experience, use the **[SP VS Code Extension](./sp-language-vscode-0.1.4.vsix)**. It provides:
- Syntax highlighting
- Hover information for built-ins and types
- Diagnostic error reporting

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

## 🗺️ Roadmap (Upcoming Features)

SP is actively evolving! Here are some of the key features planned for the near future:

- **Optional Strict Types**: TypeScript-like typing with support for grouped types for better type safety.
- **Asynchronous Programming**: Native `async`/`await` support for non-blocking operations (**Syntax TBD**).
- **Networking/HTTP**: Built-in `http` and `net` modules to enable web and network capabilities (**Syntax TBD**).
- **Enhanced Error Handling**: Improved `try`/`catch` or domain-specific error catching mechanisms (**Syntax TBD**).
- **Granular Security**: A dedicated permission system for file system and network access (**Execution TBD**).

---

## 🤝 Community & Support

- **Discord**: Join the community and find me as **SwyftPain**.
- **Email**: Reach out to **nikolamatic225@gmail.com** for professional inquiries.
- **Contributing**: Check our **[CONTRIBUTING.md](.github/CONTRIBUTING.md)** for details on how to get involved.
- **Issues**: Report bugs or request features using our dedicated **[templates](.github/ISSUE_TEMPLATE/)**.
- **Direct Support**: Check the **[Support Guide](./SUPPORT.md)** for more options.

## 💖 Sponsorship

If you find SP useful and would like to support its development, you can sponsor me via **[PayPal](https://paypal.me/mrjohnnyjo)**. Your support helps keep the project alive and growing!

---

## 📜 License

SP is released under the MIT License. See `LICENSE` for details.

---

Crafted with 💖 by **SwyftPain**
