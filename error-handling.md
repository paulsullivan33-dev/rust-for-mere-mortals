# Error Handling

**Scope:** how Rust handles failure without exceptions: `Result`,
the `?` operator, and when to panic. Builds on the `Option`/`Result`
intro in [Structs and Enums](structs-and-enums.md).

## The core idea

In many languages, errors are *invisible in the code*: a function
can throw, and you only find out by reading docs or by crashing.
In Rust, fallibility is *visible in the type*. A function returning
`Result<T, E>` is announcing "I can fail, and here's what failure
looks like." Callers must deal with it — the compiler won't let them
pretend otherwise.

## match: the explicit way

```rust
use std::fs;

fn main() {
    let result = fs::read_to_string("config.txt");
    match result {
        Ok(contents) => println!("file says: {}", contents),
        Err(e) => println!("couldn't read file: {}", e),
    }
}
```

Verbose, but nothing is hidden. You'll write this less often than
you'd think, because of `?` below.

## ?: the practical way

If your function itself returns a `Result`, `?` unwraps success or
returns the error immediately:

```rust
use std::fs;

fn load_config() -> Result<String, std::io::Error> {
    let contents = fs::read_to_string("config.txt")?;  // fails → return Err here
    Ok(contents.trim().to_uppercase())                 // succeeds → continue
}

fn main() {
    match load_config() {
        Ok(c) => println!("config: {}", c),
        Err(e) => println!("problem: {}", e),
    }
}
```

One constraint: `?` only works inside functions returning `Result`
(or `Option`). In `main`, which returns nothing, use `match`.

> Tip: `fn main() -> Result<(), Box<dyn std::error::Error>>` lets
> `main` use `?` too. You'll see this in real projects; it's fine to
> adopt once `Result` feels natural.

## unwrap and expect: the escape hatches

```rust
use std::fs;

let contents = fs::read_to_string("config.txt").unwrap();
```

`unwrap()` means "I'm sure this succeeds — crash if I'm wrong."
`expect("message")` is the same but with a custom crash message:

```rust
use std::fs;

let contents = fs::read_to_string("config.txt")
    .expect("config.txt must exist next to the binary");
```

**When they're okay:** quick prototypes, examples, tests, and cases
where failure truly can't happen (e.g. parsing `"42"`).

**When they're not:** anything other people will run. A library that
`unwrap()`s on bad input turns a recoverable error into a crash for
everyone downstream. Prefer returning `Result` and letting the
*caller* decide.

## panic!: the intentional crash

```rust
panic!("something unrecoverable happened");
```

`panic!` stops the program immediately with a message. `unwrap()` on
an `Err` panics internally — that's all it is.

Legitimate uses: asserting things that must never be false
("this list is never empty here"), unrecoverable startup failures.
Rust's philosophy: crash loudly on *bugs*, return `Result` on
*expected failures* (bad input, missing files, network errors).

## What about Option?

`?` works on `Option` too — inside functions returning `Option`:

```rust
fn first_char(s: &str) -> Option<char> {
    let c = s.chars().next()?;  // None → return None here
    Some(c.to_ascii_uppercase())
}
```

Same shape as `Result`, for "might be absent" instead of "might
fail."

## The decision table

| Situation | Do this |
|---|---|
| Prototyping, tests | `unwrap()` / `expect()` freely |
| Function can fail, caller should decide | Return `Result`, use `?` inside |
| Value might be absent | `Option`, `match` or `?` |
| Truly impossible situation | `panic!` or `unreachable!()` |
| Library code | Almost never `unwrap()` on external input |

## Check

- [ ] You can rewrite a `match` on `Result` using `?`
- [ ] You can explain when `unwrap()` is acceptable and when it isn't
- [ ] You know the difference between a bug (`panic!`) and an
      expected failure (`Result`)

Next: [Troubleshooting](troubleshooting.md).
