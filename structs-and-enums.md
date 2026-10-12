# Structs and Enums

**Scope:** grouping data with structs, modeling alternatives with
enums, and meeting `Option` and `Result` — the types that replace
null and exceptions. All examples were compiled and run.

## Structs: named bundles of data

```rust
struct User {
    name: String,
    age: u32,
    active: bool,
}

fn main() {
    let ada = User {
        name: String::from("Ada"),
        age: 36,
        active: true,
    };
    println!("{} is {}", ada.name, ada.age);
}
```

- A struct defines the *shape*; `User { ... }` builds one.
- Every field must get a value, and field order doesn't matter.
- To change a field later, the whole variable must be `mut`:

```rust
let mut ada = User { name: String::from("Ada"), age: 36, active: true };
ada.age = 37;
```

### Methods: functions attached to a struct

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let r = Rectangle { width: 3, height: 4 };
    println!("area: {}", r.area());
}
```

- `impl Rectangle` opens a block of methods for the struct.
- `&self` means "borrow the struct I'm called on" — `r.area()`
  borrows `r`, so `r` is still usable afterward.
- No classes, no inheritance: structs hold data, `impl` blocks add
  behavior. Shared behavior across types comes from *traits*
  (beyond this guide's scope — the glossary points onward).

## Enums: one of several possibilities

```rust
enum Direction {
    North,
    South,
    East,
    West,
}

let heading = Direction::North;
```

Variants can carry data:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}
```

`match` handles every variant — and the compiler *forces* you to
handle all of them:

```rust
fn describe(d: Direction) -> &'static str {
    match d {
        Direction::North => "up",
        Direction::South => "down",
        Direction::East => "right",
        Direction::West => "left",
    }
}
```

Leave out a variant and the compiler errors: "non-exhaustive
patterns." You cannot forget a case. This is one of Rust's quiet
superpowers.

## Option: Rust's answer to null

There is no `null` in Rust. "Might not have a value" is `Option`:

```rust
enum Option<T> {   // (simplified — it's in the standard library)
    Some(T),
    None,
}
```

```rust
fn find_user(id: u32) -> Option<String> {
    if id == 1 {
        Some(String::from("Ada"))
    } else {
        None
    }
}

fn main() {
    match find_user(1) {
        Some(name) => println!("found {}", name),
        None => println!("no such user"),
    }
}
```

You can't accidentally use a possibly-missing value — the compiler
makes you handle `None` before touching the `Some`. An entire
category of "null pointer" crashes becomes impossible.

Shortcuts you'll see (same idea, less typing):

```rust
let name = find_user(1).unwrap_or(String::from("guest"));
```

## Result: Rust's answer to exceptions

There are no exceptions either. "Might fail" is `Result`:

```rust
enum Result<T, E> {   // (simplified)
    Ok(T),
    Err(E),
}
```

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("can't divide by zero"))
    } else {
        Ok(a / b)
    }
}
```

The `?` operator propagates errors upward — "return early if this
failed":

```rust
fn read_number() -> Result<i32, String> {
    let text = "42";
    let n: i32 = text.parse().map_err(|_| String::from("not a number"))?;
    Ok(n * 2)
}
```

If `parse()` fails, `?` returns the `Err` immediately. If it
succeeds, execution continues with the unwrapped value. Error
handling is visible in the code, not hidden in a `catch` block
three functions away.

## Check

- [ ] You can build a struct, read its fields, and add a method
- [ ] You can explain why `match` must cover every enum variant
- [ ] You can say what replaces `null` (`Option`) and exceptions
      (`Result`) — and why that's safer

Next: [Error Handling](error-handling.md) goes deeper on `Result`.
