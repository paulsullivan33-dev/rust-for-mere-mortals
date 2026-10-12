# Variables and Types

**Scope:** declaring variables, mutability, shadowing, and the types
you'll use 95% of the time. All examples were compiled and run.

## Declaring variables

```rust
let age = 42;
let name = "Ada";
```

`let` introduces a variable. Rust figures out the type itself
(**type inference**) — you rarely write types down.

### Immutable by default

```rust
let x = 5;
// x = 6;  // ERROR: cannot assign twice to immutable variable
```

To allow change, say `mut`:

```rust
let mut x = 5;
x = 6;  // fine
```

This is deliberate: when you read someone's code (or your own, six
months later), a variable *without* `mut` is guaranteed never to
change. That's information the compiler enforces.

### Shadowing: same name, new variable

```rust
let x = 5;
let x = x + 1;      // a NEW variable x, shadowing the old one
let x = "five";     // even the type can change
```

Shadowing (`let` again with the same name) is different from
mutation: the old variable still exists underneath, unchanged, but
the name now points to the new one. It's commonly used to transform
a value step by step:

```rust
let input = "  42  ";
let input = input.trim();
let input: i32 = input.parse().unwrap();
```

## The types you'll actually use

| Type | Meaning | Example |
|---|---|---|
| `i32` | 32-bit integer (the default) | `let n = 42;` |
| `i64` | 64-bit integer | `let big: i64 = 9_000_000_000;` |
| `u32` | Unsigned (no negatives) 32-bit | `let count: u32 = 7;` |
| `f64` | 64-bit decimal (the default) | `let pi = 3.14159;` |
| `bool` | true/false | `let done = false;` |
| `char` | One character (Unicode) | `let c = '🦀';` |
| `String` | Growable text (more in [Ownership](ownership.md)) | `let s = String::from("hi");` |
| `&str` | Text slice — a view into text | `let s = "hi";` |

A few notes:

- **Integers:** `i32` vs `u32` vs `i64`... the letter is signed
  (`i`) or unsigned (`u`), the number is bits. Default integer
  type is `i32`; default decimal type is `f64`. Underscores in
  numbers are ignored: `1_000_000` reads better than `1000000`.
- **`char` vs `&str` vs `String`:** single quotes are one character
  (`'a'`), double quotes are text (`"a"`). Rust has *two* text
  types — this confuses everyone at first and is explained properly
  in [Ownership](ownership.md).
- **Tuples** group values: `let t = (42, "Ada", true);` — access
  with `t.0`, `t.1`, `t.2`.
- **Arrays** are fixed-size: `let a = [1, 2, 3];` — always exactly
  3 elements. (Growable lists are `Vec`, covered later.)

## Constants

```rust
const MAX_USERS: u32 = 100;
```

`const` values never change, must have their type written out, and
can live anywhere (even outside functions). Use them for magic
numbers you'd otherwise scatter through the code.

## Type annotations: when to write them

You *can* always write the type:

```rust
let x: i32 = 5;
```

You usually don't need to. Write them when:

- The compiler asks you to (it will, clearly)
- Parsing text into a number: `let n: i32 = "42".parse().unwrap();`
- A reader would otherwise guess wrong

## Check

- [ ] You can explain `let` vs `let mut` vs shadowing
- [ ] You know the default integer (`i32`) and decimal (`f64`) types
- [ ] You can say why Rust has both `String` and `&str` (short
      version: one owns its text, the other borrows a view — full
      story in [Ownership](ownership.md))

Next: [Ownership](ownership.md) — the chapter everything else
builds on.
