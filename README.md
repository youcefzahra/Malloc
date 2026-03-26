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

## 🎯 Testing the Allocator

A pre-compiled shared library is available in the **Releases** section.

1. Download the `malloc_demo.tar.gz` archive.
2. Use `LD_PRELOAD` to run any command with your custom allocator:
```bash
# Example: Running 'ls' with your custom malloc
LD_PRELOAD=./libmalloc.so ls -la
```
