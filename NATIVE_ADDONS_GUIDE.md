# 🌌 SP Native Addons: Cross-Language Guide

Welcome to the comprehensive guide for building native addons for the **SP Programming Language**! While SP is built in C++, its minimalist ABI and C-style argument passing allow you to use almost any language that supports C linkage.

---

## 🏗️ Core Concepts

To interface with SP, you need to understand two key structures:

### 1. `sp_value` (NaN-Boxing)
SP uses **NaN-boxing** for values. Every SP value is a 64-bit unsigned integer.
- **Numbers**: Standard `double` (64-bit float).
- **Other Types**: Boxed with a specific NaN-pattern and tag.

### 2. `sp_args` (`std::vector` Layout)
Arguments are passed as a reference to a C++ `std::vector<Value>`. In most modern compilers (GCC/Clang), this corresponds to three pointers: `start`, `finish`, and `end_of_storage`.

> [!TIP]
> Use our [**`sp_addon.h`**](./include/sp_addon.h) for the easiest C/C++ development!

---

## ⚡ Language Examples

The following examples demonstrate how to implement a basic `add_numbers` function in various languages. Since SP uses a stable C-style ABI, you can use any language that can export C symbols.

### 📑 Table of Contents
- [⚡ Zig](#-zig)
- [🦀 Rust](#-rust)
- [🐹 Go](#-go)
- [🏺 Odin](#-odin)
- [👑 Nim](#-nim)
- [💎 Crystal](#-crystal)
- [🍎 Swift](#-swift)
- [🐉 D](#-d)
- [👴 FreePascal](#-freepascal)

---

### ⚡ Zig
Zig's memory-safe and low-level features make it an excellent choice for SP addons.

```zig
const std = @import("std");

pub const Value = extern struct { bits: u64 };
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
    if (args.len() < 2) return 0;
    const v1 = args.start[0].bits;
    const v2 = args.start[1].bits;
    _ = v1; _ = v2;
    return 0; // Placeholder
}
```
**Compile:** `zig build-lib -dynamic addon.zig -femit-bin=libmy_addon.so`

---

### 🦀 Rust
Rust is powerful and safe. Use `cdylib` and `#[no_mangle]`.

```rust
#[repr(C)]
pub struct Value { bits: u64 }

#[repr(C)]
pub struct StdVector {
    start: *const Value,
    finish: *const Value,
    end_of_storage: *const Value,
}

#[no_mangle]
pub unsafe extern "C" fn add_numbers(_i: *mut (), args: *const StdVector) -> u64 {
    let args = &*args;
    // ...
}
```
**Compile:** `cargo build --release`

---

### 🐹 Go
Go can create addons using `cgo`.

```go
package main

/*
#include <stdint.h>
typedef struct { uint64_t bits; } Value;
typedef struct { Value* s; Value* f; Value* e; } StdVector;
*/
import "C"
import "unsafe"

//export add_numbers
func add_numbers(interp unsafe.Pointer, args *C.StdVector) C.uint64_t {
	// ...
}
```
**Compile:** `go build -o libmy_addon.so -buildmode=c-shared main.go`

---

### 🏺 Odin
Odin's simple FFI makes it a great fit.

```odin
package main

Value :: struct { bits: u64 }
StdVector :: struct { start, finish, end: ^Value }

@(export)
add_numbers :: proc "c" (interp: rawptr, args: ^StdVector) -> u64 {
    // ...
}
```
**Compile:** `odin build addon.odin -build-mode:shared -out:libmy_addon.so`

---

### 👑 Nim
Nim is incredibly flexible and compiles to C.

```nim
type
  Value = object
    bits: uint64
  StdVector = object
    start, finish, storage: ptr Value

proc add_numbers(interp: pointer, args: ptr StdVector): uint64 {.exportc, dynlib, cdecl.} =
  # ...
```
**Compile:** `nim c --app:lib -d:release addon.nim`

---

### 💎 Crystal
Crystal is fast and Ruby-like.

```crystal
lib SP
  struct Value; bits : UInt64; end
  struct StdVector; start, finish, storage : Value*; end
end

fun add_numbers(interp : Void*, args : SP::StdVector*) : UInt64
  # ...
end
```
**Compile:** `crystal build --shared addon.cr`

---

### 🍎 Swift
Swift's `@_cdecl` makes it easy to export symbols.

```swift
struct Value { var bits: UInt64 }
struct StdVector { var start, finish, storage: UnsafePointer<Value> }

@_cdecl("add_numbers")
public func add_numbers(interp: UnsafeMutableRawPointer?, args: UnsafePointer<StdVector>?) -> UInt64 {
    // ...
}
```
**Compile:** `swiftc -emit-library -o libmy_addon.so addon.swift`

---

### 🐉 D
D's `extern(C)` provides first-class interop.

```d
import std.stdio;

struct Value { ulong bits; }
struct StdVector { Value* start, finish, storage; }

extern(C) ulong add_numbers(void* interp, StdVector* args) {
    // ...
}
```
**Compile:** `ldc2 -shared addon.d -of=libmy_addon.so`

---

### 👴 FreePascal
Classic, fast, and simple.

```pascal
type
  PValue = ^TValue;
  TValue = record bits: QWord; end;
  PStdVector = ^TStdVector;
  TStdVector = record start, finish, storage: PValue; end;

function add_numbers(interp: Pointer; args: PStdVector): QWord; cdecl; export;
begin
  // ...
end;
```
**Compile:** `fpc -Mdelphi -Tlinux -Pauto -Wl-shared addon.pas`

---

### 🛠️ Compiling & Linking Addons
When compiling your addon, there are a few critical requirements:

1.  **Shared Library**: The output MUST be a shared object (`.so`).
2.  **Position Independent Code**: You must use `-fPIC` (C/C++) or equivalent options.
3.  **No Linking Required**: Because the SP interpreter is self-contained, your addon **does not need to link against the SP binary** at compile-time. Symbols are resolved dynamically when the addon is loaded.

> [!TIP]
> Always ensure your shared object starts with the `lib` prefix (e.g., `libmath.so`) to be correctly discovered by the `use` statement in SP.

---

Happy Coding with **SP**! 
