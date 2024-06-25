---
title: Stanford CS110L
date: 2024-01-21
draft: false
math: true
description: 
categories: 
tags:
  - OS
  - Computer-Security
  - Programming-Language
---


> [!info] 
> 
> Study notes based on [Stanford CS110L, Spring 2020](https://reberhardt.com/cs110l/spring-2020/) online videos.

## Why Rust

[lecture-01.pdf](https://reberhardt.com/cs110l/spring-2020/slides/lecture-01.pdf)

- Why not C/C++?
    - Memory safety issues
        - Overflow: buffer overflow, integer overflow, etc.
        - Valgrind is unable to find leaks resulted by buffer overflow.
- Why not use GC'ed languages
    - Expensive
        - No matter what type of GC is used, there will always be nontrivial memory overhead.
    - Disruptive
        - Drop whatever you're doing - it's time for GC!
    - Non-deterministic
        - Nobody knows when will the next GC pause be. Depends on how much money is being used.
    - Precludes manual optimization
        - GC optimizes for the average use case, ignorant of knowing how you might use memory.
    - Memory safety issues persist

## Memory Safety

[lecture-02.pdf](https://reberhardt.com/cs110l/spring-2020/slides/lecture-02.pdf)

- Why is it so easy to screw up in C?
    - Dangling pointers
    - Double frees
    - Iterator Invalidation
    - Memory leaks
- It is incredibly hard to reason about programs.
- How does Rust prevent from the errors above?
    - Ownership
        - Passing ownership/references: just passes a pointer
    - Borrowing
    - Lifetimes

## Error Handling

[lecture-03.pdf](https://reberhardt.com/cs110l/spring-2020/slides/lecture-03.pdf)

- Handling nulls
    - Delete `NULL` and replace with `None` wrapped in `Option`
- Handling errors
    - Most languages use exceptions
        - Failure mode are hard to spot: any function can throw any exception at any time
        - Hard to manage in evolving codebases
        - Especially hard when manual memory management is involved
    - Error handling in Rust
        - If an unrecoverable error occurs, panic
        - If a recoverable error may occur, return a `Result`
        - Use `unwrap()` and `expect()`

## Object Oriented Rust

Check out the official [lecture notes](https://reberhardt.com/cs110l/spring-2020/lecture-notes/).

## Traits and Generics

[lecture-05.pdf](https://reberhardt.com/cs110l/spring-2020/slides/lecture-05.pdf)

- Traits
    - What can traits do?
        - Display
        - Clone/Copy
        - Iterator/IntoIterator
        - Eq/PartialEq
    - Allows us to override functionality
        - Drop
        - Deref
    - Allows us to define default implementations
        - ToString
    - Allows us to overload operators
- Generics
    - No performance impact at runtime: compiler create separate functions for types that are used

## Smart Pointers

[lecture-06.pdf](https://reberhardt.com/cs110l/spring-2020/slides/lecture-06.pdf)

- `Box<T>`
    - Plays the same role as  `unique_ptr<T>` in C++
- `Rc<T>`
    - Allows us to have multiple immutable references to a chunk of heap memory
    - Caution: you can have memory leaks if you create reference cycles
- `RefCell<T>`
    - Allows us to have shared references to the cell with mutability
    - Its `new` function doesn't heap allocate
    - This is still safe because it will enforce the reference rules at runtime, which results in additional cost
    - Common pattern: `Rc<RefCell<T>>`

## Pitfalls in Multiprocessing

[lecture-07.pdf](https://reberhardt.com/cs110l/spring-2020/slides/lecture-07.pdf)

- Don't call `fork()`
    - Why fork?
        - Get concurrent execution
            - Accidentally nesting forks when spawning multiple child processes
            - Children can execute code they weren't supposed to
            - Accessing data structure during threading
            - Failure to clean up zombie children if `waitpid()` isn't called
        - Invoke external functionality on the system
            - Almost every `fork()` is followed by an `exec()`
    - Common multiprocessing tactic
        - Let `fork()` and `exec()` be
        - Define a higher-level abstraction to take care of the common cases
    - `Command` in Rust
        - No concurrency: run,  and get the output in a buffer
        - No concurrency: run (without swallowing output), and get the status code
        - With concurrency: spawn and immediately return
        - `pre_exec()` function
- Don't call `pipe()`
    - Problems
        - Leaked file descriptors
        - Use-after-close
    - Potential solution
        - Add a layer of abstraction
        - Write to a stdin pipe (what rust does)
            - The `os_pipe` crate allows for creating arbitrary pipes

## Google Chrome

[lecture-08.pdf](https://reberhardt.com/cs110l/spring-2020/slides/lecture-08.pdf)

- Don't call `signal()`
    - `signal()` is dead. Long live `sigaction()`
        - The only portable use of `signal()` is to set a signal's disposition to `SIG_DFL` or `SIG_IGN`