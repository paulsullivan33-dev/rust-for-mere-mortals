# Ownership

**Scope:** Rust's three memory rules, what "move" means, and why
`String` and `&str` are different types. This is the chapter the
rest of the language builds on. Read it twice if needed — everyone
does.

## The idea in one paragraph

Every piece of data in a Rust program has exactly one **owner**
(a variable). When the owner goes out of scope, the data is freed —
automatically, at a point the compiler chooses. There is no garbage
collector sweeping up later, and no manual `free()` to forget.
Because ownership is tracked at compile time, the compiler can prove
your program never uses freed memory.

## The three rules

1. Each value has exactly one owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value is dropped (freed).

That's the whole system. Everything below is consequences.

## Rule 3 in action: scope ends, memory freed

```rust
{
    let s = String::from("hello");  // s owns the string
    println!("{}", s);
}  // s goes out of scope here → the string is freed, automatically
// println!("{}", s);  // ERROR: s no longer exists
```

Compare with C, where you'd call `free()` yourself (and might
forget), or Python, where a garbage collector frees it "eventually."
Rust frees it *right here*, deterministically, with zero runtime
cost.

## Rule 2 in action: moving, not copying

```rust
let s1 = String::from("hello");
let s2 = s1;  // s1 is MOVED into s2
// println!("{}", s1);  // ERROR: s1 is no longer valid
println!("{}", s2);  // fine
```

After `let s2 = s1`, there is one owner: `s2`. Using `s1` afterward
is a compile error. This prevents double-free: if both `s1` and
`s2` thought they owned the string, both would try to free it when
their scopes ended.

This surprises people coming from Python or Java, where assignment
copies a reference and both names keep working. In Rust, assignment
of an owned value **moves** it.

**Cheap types are the exception:** integers, booleans, chars, and
other small fixed-size types implement `Copy` — assigning them
duplicates the bits, and both names stay valid:

```rust
let a = 5;
let b = a;  // copies the 5; a still works
println!("{} {}", a, b);  // fine
```

Rule of thumb: if it's a simple value that fits in a few bytes, it
copies. If it manages memory on the heap (like `String`), it moves.

## Functions and ownership

Passing a value to a function moves it, just like assignment:

```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s);   // s moved into the function
    // println!("{}", s);  // ERROR: s was moved
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string);
}  // some_string goes out of scope → freed here
```

And functions can give ownership back:

```rust
fn main() {
    let s1 = gives_ownership();      // function moves its return into s1
    let s2 = String::from("hello");
    let s3 = takes_and_gives_back(s2);  // s2 moved in, return moved out
}

fn gives_ownership() -> String {
    String::from("hello")
}

fn takes_and_gives_back(a_string: String) -> String {
    a_string
}
```

Yes, this gets tedious — which is exactly why
[Borrowing](borrowing.md) exists. Ownership is the strict default;
borrowing is the convenient escape hatch.

## Why `String` and `&str` are different

Now the puzzle from [Variables and Types](variables-and-types.md)
resolves:

| | `String` | `&str` |
|---|---|---|
| Owns its text? | Yes | No — it's a *view* into someone else's text |
| Growable? | Yes (`push_str`) | No |
| Freed when? | When its owner goes out of scope | Never frees — it doesn't own anything |
| Literal `"hi"` is... | — | `&str` (baked into the program) |

```rust
let owned: String = String::from("hello");  // I own this text
let view: &str = "hello";                    // a view of text in the binary
let view2: &str = &owned;                    // a view of owned's text
```

The `&` means "borrowed" — the full story is
[Borrowing](borrowing.md).

## Check

- [ ] You can state the three ownership rules from memory
- [ ] You can explain why `let s2 = s1;` makes `s1` unusable (for
      a `String`) but `let b = a;` keeps `a` usable (for an `i32`)
- [ ] You can explain, in one sentence, the difference between
      `String` and `&str`

Next: [Borrowing](borrowing.md) — how to use values without taking
ownership of them.
