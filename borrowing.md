# Borrowing

**Scope:** references (`&`), the one rule about mutable borrows, and
why the compiler sometimes complains about code that looks fine.
Builds directly on [Ownership](ownership.md).

## The idea in one paragraph

Moving ownership on every function call would be exhausting. So Rust
lets you **borrow** a value: hand out a reference (`&`) that lets
code *use* the data without owning it. The owner keeps ownership;
the borrower promises to give it back — a promise the compiler
enforces.

## Immutable borrows: look, don't touch

```rust
fn main() {
    let s = String::from("hello");
    let len = calculate_length(&s);  // borrow s; s stays mine
    println!("'{}' is {} long", s, len);  // s still usable!
}

fn calculate_length(s: &String) -> usize {
    s.len()
}  // s (the borrow) goes out of scope; nothing is freed —
   // the borrow never owned anything
```

`&s` creates a reference to `s`. The function takes `&String` — a
borrowed string. When the function ends, the borrow evaporates; the
original `s` is untouched and still owned by `main`.

## Mutable borrows: touch, but exclusively

```rust
fn main() {
    let mut s = String::from("hello");
    change(&mut s);           // mutable borrow
    println!("{}", s);        // "hello, world"
}

fn change(s: &mut String) {
    s.push_str(", world");
}
```

`&mut` lets the borrower modify the data. But there's a strict rule:

> **At any moment, you may have EITHER any number of immutable
> borrows OR exactly one mutable borrow. Never both.**

```rust
let mut s = String::from("hello");

let r1 = &s;      // immutable borrow — fine
let r2 = &s;      // another immutable borrow — fine
// let r3 = &mut s;  // ERROR: can't borrow mutably while immutably borrowed

println!("{} {}", r1, r2);
```

And:

```rust
let mut s = String::from("hello");

let r1 = &mut s;  // one mutable borrow — fine
// let r2 = &mut s;  // ERROR: only one mutable borrow at a time
// let r3 = &s;      // ERROR: no immutable borrows during a mutable one

println!("{}", r1);
```

**Why this rule exists:** it's the data-race killer. If one piece of
code is writing while another is reading, you get unpredictable
results. By forbidding that combination at compile time, Rust
eliminates data races — not just in single-threaded code, but across
threads too.

## The classic beginner error

```rust
let mut s = String::from("hello");

let r1 = &s;          // immutable borrow starts
s.push_str(", world"); // ERROR: can't mutate while borrowed
println!("{}", r1);
```

The compiler says something like "cannot borrow `s` as mutable
because it is also borrowed as immutable." Translation: `r1` is still
in use (it's printed later), so mutating `s` now could invalidate
what `r1` sees. Fix: finish with the borrow first.

```rust
let mut s = String::from("hello");

let r1 = &s;
println!("{}", r1);   // borrow's last use is here...
s.push_str(", world"); // ...so this is now fine
```

The compiler tracks borrows to their *last use*, not to the end of
the block. Once `r1` is done, the borrow ends and mutation is
allowed. (This smarter tracking is called *non-lexical lifetimes* —
you don't need the name, just the behavior.)

## Dangling references are impossible

```rust
fn dangle() -> &String {  // ERROR: missing lifetime specifier
    let s = String::from("hello");
    &s
}  // s is freed here — the reference would point at freed memory
```

In C this compiles and explodes at runtime. In Rust it's a compile
error: the compiler sees that `s` dies at the end of the function
while the returned reference would outlive it. The fix is to return
the owned `String` instead of a reference.

## Borrowing cheat sheet

| You want to... | Take... | Example |
|---|---|---|
| Read a value without owning it | `&T` | `fn len(s: &String)` |
| Modify a value without owning it | `&mut T` | `fn push(s: &mut String)` |
| Keep it after the call | Nothing — take ownership | `fn eat(s: String)` |

## Check

- [ ] You can state the borrow rule (many immutable XOR one
      mutable) from memory
- [ ] You can explain why `&mut s` is rejected while `&s` is in use
- [ ] You can fix the "cannot borrow as mutable" error by reordering
      code

Next: [Structs and Enums](structs-and-enums.md).
