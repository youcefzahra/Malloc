# 🧠 Malloc - Custom Memory Allocator

> **Showcase Repository:** To comply with EPITA's anti-plagiarism regulations, the source code of this library is kept private. This repository serves as a technical showcase documenting the implementation of a dynamic memory allocator.

## 📖 Project Overview

**Malloc** is a systems programming project that involves re-implementing the standard C dynamic memory allocation library (`libmalloc.so`). The goal is to manage the heap memory by interfacing directly with the operating system using low-level system calls like `mmap` and `munmap`.

## ✨ Implemented Features

* **Core Functions:** Complete implementation of `malloc`, `free`, `calloc`, and `realloc`.
* **Alignment:** Ensures that all returned memory areas are strictly aligned on `long double` for optimal performance and compatibility.
* **Thread-Safety:** Fully safe for multi-threaded environments using `pthread` mutexes and spinlocks to prevent data corruption during simultaneous allocations.
* **Memory Optimization:** Advanced management of the memory footprint through fragmentation reduction and immediate reusability of freed blocks.
* **Smart Realloc:** Optimized `realloc` that attempts to extend existing memory blocks in-place whenever possible to avoid unnecessary copies.

## 🏗️ Technical Highlights

### System Call Interface

Unlike basic implementations, this allocator avoids calling `mmap` for every small allocation to prevent performance degradation. It manages memory pages efficiently and ensures that empty pages are properly unmapped to return memory to the system.

### Dynamic Overriding

The library is designed to be "preloaded" into existing binaries (like `ls`, `cat`, or even `vlc`), effectively replacing the standard libc allocator at runtime.

### 🔍 Symbol Export Verification

To ensure the library remains lightweight and strictly follows the required API, only the four mandatory symbols are exported. This can be verified with:
```bash
nm -C -D libmalloc.so
```

* **Global Symbols (T):** Only `malloc`, `free`, `calloc`, and `realloc` appear as public symbols.
* **Hidden Implementation:** No internal auxiliary functions are exposed, proving clean encapsulation and proper use of visibility flags.

## 🎯 Testing the Allocator

A pre-compiled shared library is available in the **Releases** section.

1. Download and extract the `malloc_demo.tar.gz` archive:
```bash
tar -xvf malloc_demo.tar.gz
cd malloc_demo
```

2. Use `LD_PRELOAD` to run any command with the custom allocator:
```bash
LD_PRELOAD=./libmalloc.so ls -la
```
