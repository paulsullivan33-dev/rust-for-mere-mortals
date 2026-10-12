# Glossary

Quick definitions, A to Z. Each term is explained in depth in its
guide; this is the reminder card.

- **Borrow** — using a value through a reference (`&`) without taking
  ownership. See [Borrowing](borrowing.md).
- **Borrow checker** — the compiler pass that enforces the ownership
  and borrowing rules. Not a separate program; just what people call
  that part of `rustc`.
- **Cargo** — Rust's build tool and package manager: builds, runs,
  tests, and fetches libraries. See [Your First Program](first-program.md).
- **Crate** — Rust's word for a library or package (like a Python
  package or npm module).
- **Edition** — a version of the language rules (2015, 2018, 2021,
  2024). Set in `Cargo.toml`; new editions can change syntax without
  breaking old code.
- **Enum** — a type with a fixed set of variants, each optionally
  carrying data. See [Structs and Enums](structs-and-enums.md).
- **Immutable** — unchangeable. Variables are immutable unless
  declared `mut`.
- **Lifetime** (`'a`) — a compiler annotation saying how long a
  reference is valid. Mostly inferred; written out when returning
  borrows from functions.
- **Macro** (`println!`) — code that writes code at compile time.
  The `!` marks the call site.
- **match** — exhaustive pattern matching over enums (and more).
  The compiler requires every case handled.
- **Move** — transferring ownership: after `let b = a;` (for owned
  types), `a` is no longer usable. See [Ownership](ownership.md).
- **Mutable** (`mut`) — allowed to change. Must be declared;
  the default is immutable.
- **Option** — `Some(value)` or `None`. Replaces null.
- **Owner** — the one variable that owns a value; when it goes out
  of scope, the value is freed.
- **Panic** — an unrecoverable crash, intentional (`panic!`) or via
  `unwrap()` on an error.
- **Reference** (`&`) — a borrow: a pointer the compiler guarantees
  is valid. `&T` reads, `&mut T` reads and writes (exclusively).
- **Result** — `Ok(value)` or `Err(error)`. Replaces exceptions.
- **rustc** — the Rust compiler itself. You rarely call it directly;
  Cargo does.
- **rustup** — the toolchain installer/updater. See
  [Installing Rust](installing-rust.md).
- **Shadowing** — declaring a new variable with the same name via
  `let`, hiding the old one.
- **Slice** (`&str`, `&[u8]`) — a borrowed view into a contiguous
  sequence. Doesn't own the data.
- **String** — owned, growable text. `&str` is the borrowed view.
- **Struct** — a named bundle of fields. Behavior attaches via
  `impl` blocks.
- **Trait** — shared behavior across types (like an interface).
  Beyond these guides' scope; the
  [Rust Book](https://doc.rust-lang.org/book/) covers it well.
- **`?` operator** — unwrap-or-return-early for `Result`/`Option`.
  See [Error Handling](error-handling.md).
