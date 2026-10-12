# Rust for Mere Mortals

Plain-language guides to the Rust programming language, written for
people who want memory safety without giving up control — no CS degree
required.

No jargon without an explanation. No "it's simple, just..." Every
concept starts with the *idea*, then the code.

One honest note up front: Rust is stricter than Python and stricter
than C in a different way. The compiler will reject programs that
other languages happily run. That's the point — it catches the bug
before your users do — but the first week can feel like arguing with
a very stubborn, very correct friend. These guides respect that
instead of hiding it.

## Start here

1. [What Rust Is](what-rust-is.md) — what it is, where it lives, and
   why learn it in the first place.
2. [Installing Rust](installing-rust.md) — Windows, Mac, and Linux
   via rustup, with checks that it worked.
3. [Your First Program](first-program.md) — the edit-build-run cycle
   with Cargo, with checks along the way.

## The building blocks

4. [Variables and Types](variables-and-types.md) — `let`, `mut`,
   shadowing, and the types you'll actually use.
5. [Ownership](ownership.md) — the big one: Rust's memory rules,
   explained plainly.
6. [Borrowing](borrowing.md) — references, and the one rule about
   mutable borrows.
7. [Structs and Enums](structs-and-enums.md) — modeling your own
   data, including `Option` and `Result`.
8. [Error Handling](error-handling.md) — no exceptions: how Rust
   makes you deal with failure up front.

## Reference

9. [Troubleshooting](troubleshooting.md) — borrow checker errors and
   other compiler complaints, plain fixes.
10. [Glossary](glossary.md) — quick definitions, A to Z.

## Writing criteria

Every guide follows the same [writing criteria](criteria.md): plain
words, no hype, honest limits, and checkable steps.
