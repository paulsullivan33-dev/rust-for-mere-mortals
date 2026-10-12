# What Rust Is

**Scope:** what the language is, where it's used, and whether it's
worth your time. Not a tutorial — that starts with
[Your First Program](first-program.md).

## The one-sentence version

Rust is a programming language that runs as fast as C, but the
compiler refuses to build programs with certain kinds of memory bugs.

## The problem it solves

Most languages pick one side of a tradeoff:

| Approach | Example | Cost |
|---|---|---|
| Trust the programmer | C, C++ | Memory bugs: crashes, security holes |
| Watch at runtime | Python, Java, Go | Garbage collector pauses, slower, more memory |

Rust takes a third path: **check at compile time.** The compiler
tracks which part of your program owns each piece of memory, and
rejects code that could use memory after freeing it, free it twice,
or let two threads fight over it. No garbage collector running in
the background. The checking happens once, when you build.

The practical effect: whole categories of bugs become *compile
errors* instead of *mystery crashes at 2 AM*.

## Where Rust actually lives

- **Operating systems:** parts of the Linux kernel and Windows kernel
  are now written in Rust; Android uses it for security-critical
  components.
- **Browsers:** Firefox's CSS engine (Stylo) is Rust.
- **Infrastructure:** Cloudflare, Discord, and Dropbox run Rust in
  performance-critical backends.
- **Developer tools:** `ripgrep` (fast file search), `uv` (fast
  Python package installer), and much of the modern Rust-adjacent
  toolchain are Rust programs.
- **Embedded:** microcontrollers, where there's no room for a garbage
  collector.
- **WebAssembly:** Rust compiles well to WASM for browser-side speed.

## What Rust is not

- **Not a scripting language.** There's no REPL-style workflow like
  Python's. You write a file, compile it, run it. The cycle is fast,
  but it's a compiled language.
- **Not garbage-collected.** There's no background cleaner. Memory is
  freed at predictable, compiler-determined points.
- **Not object-oriented in the Java sense.** No classes, no
  inheritance. It has structs, traits (shared behavior), and enums —
  closer to "data plus functions" than to class hierarchies.
- **Not easy at first.** The borrow checker (covered in
  [Ownership](ownership.md) and [Borrowing](borrowing.md)) rejects
  code that *looks* fine. Expect a few weeks of friction. This is
  normal and temporary.

## Should you learn it?

Worth it if you:

- Write or maintain C/C++ and want fewer memory bugs
- Build CLIs, servers, or embedded firmware where speed matters
- Want to understand what "memory safety" actually means, concretely

Probably overkill if you:

- Mostly glue APIs together in Python or JavaScript
- Need a quick script this afternoon (use Python)
- Want the gentlest possible first language (Python is still that)

## Check

You can explain, in your own words:

- [ ] What the borrow checker checks (who owns each piece of memory)
- [ ] One place Rust is used in the real world
- [ ] One thing Rust is *not* good for

Next: [Installing Rust](installing-rust.md).
