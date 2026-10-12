# Troubleshooting

**Scope:** the compiler errors you'll actually hit, what they mean,
and the plain fix for each. Rust's errors are famously helpful —
read them fully before changing code; the answer is usually in the
message.

## Borrow checker errors

### "cannot borrow as mutable because it is also borrowed as immutable"

```rust
let mut s = String::from("hi");
let r = &s;
s.push_str("!");   // ERROR
println!("{}", r);
```

**Meaning:** `r` is still in use (printed later), so mutating `s`
now could invalidate it.

**Fix:** finish with the borrow before mutating — move the
`println!` above the `push_str`, or drop `r` earlier. See
[Borrowing](borrowing.md).

### "use of moved value"

```rust
let s = String::from("hi");
takes(s);
println!("{}", s);  // ERROR: value used after move
```

**Meaning:** ownership moved into `takes(s)`; `s` is gone.

**Fixes (pick one):**
- Pass a borrow instead: `takes(&s)` (change the function to take `&String`)
- Clone if you genuinely need two copies: `takes(s.clone())`
  (costs a copy — fine for small data, think twice for big)
- Have the function return ownership back

### "cannot move out of borrowed content"

You're trying to move something out from behind a reference.

**Fix:** usually `clone()` the piece you need, or restructure so
you own the data first.

## Type errors

### "mismatched types ... expected `i32`, found `&str`"

You gave a function or variable the wrong kind of value. Often it's
the `String` vs `&str` confusion:

```rust
fn greet(name: &str) { println!("hi {}", name); }
let owned = String::from("Ada");
greet(&owned);  // borrow it — &String becomes &str automatically
```

**Fix:** add or remove `&` / `String::from` at the boundary. The
compiler points at the exact line.

### "no method named X found"

**Common causes:**
- Calling a method on the wrong type (check with the compiler's
  "help" note — it often suggests the right one)
- Forgetting `mut` on something you then try to mutate through a
  method like `push_str`
- Missing trait import (e.g. some iterator methods need
  `use std::...` — the error usually names it)

## Lifetime errors

### "missing lifetime specifier" / lifetime complaints

```rust
fn longest(a: &str, b: &str) -> &str {  // ERROR
```

**Meaning:** the function returns a borrow, but the compiler can't
tell *which input* it borrows from. It needs you to say.

**Fix:** add lifetime annotations (the `'a` syntax):

```rust
fn longest<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() { a } else { b }
}
```

Read `'a` as "some lifetime, same one everywhere it appears."
You're promising the returned reference lives as long as the inputs
do. This is the most advanced topic in these guides — if lifetimes
feel murky, that's normal; most daily Rust code needs them rarely,
because the compiler fills in the simple cases itself (*lifetime
elision*).

## Build/tooling issues

### `cargo` not found after installing

Your terminal doesn't have the Cargo directory on its `PATH`.
Close and reopen the terminal, or run the `source` line the
rustup installer printed (usually
`source "$HOME/.cargo/env"`).

### "linker not found" on Linux

Rust needs a C linker. Install it:

```sh
# Debian/Ubuntu
sudo apt install build-essential
# Fedora
sudo dnf install gcc
```

### Slow first build

Normal. Rust compiles dependencies from scratch the first time;
later builds reuse them and are much faster. `cargo check` during
editing is quicker than `cargo build`.

## The meta-rule

When the compiler rejects your code, it's not being difficult —
it's reporting a bug it *proved* exists, before your users find it.
Read the full error (including the "help:" and "note:" lines —
they're genuinely useful), fix what it points at, and move on.
Fighting it for an hour usually means the design needs a small
rethink, not a clever workaround.

## Check

- [ ] You can fix "use of moved value" three different ways
- [ ] You know what to try first for "mismatched types"
- [ ] You know what `'a` means when you see it

Next: [Glossary](glossary.md).
