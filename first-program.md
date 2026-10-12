# Your First Program

**Scope:** creating a Rust project with Cargo, building it, running
it, and reading the files Cargo made. All commands below were run
and verified.

## Make a project

```sh
cargo new hello
cd hello
```

Cargo created a folder with this inside:

```
hello/
├── Cargo.toml        # project settings: name, version, libraries
└── src/
    └── main.rs       # your code
```

Look at `src/main.rs`:

```rust
fn main() {
    println!("Hello, world!");
}
```

- `fn main()` — the function that runs when the program starts.
- `println!` — prints a line. The `!` means it's a *macro* (a
  code-writing shortcut), not a regular function. You'll see `!`
  on a few built-in helpers; for now, just read it as "print line."

`Cargo.toml` holds the project's name, version, and edition:

```toml
[package]
name = "hello"
version = "0.1.0"
edition = "2021"
```

You won't touch this file much at first.

## Build and run

```sh
cargo run
```

**Check:** you should see something like:

```
   Compiling hello v0.1.0 (/path/to/hello)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.42s
     Running `target/debug/hello`
Hello, world!
```

`cargo run` compiles *and* runs. Two related commands:

| Command | Does | Use when |
|---|---|---|
| `cargo build` | Compiles only | You want the binary without running it |
| `cargo run` | Compiles (if needed) and runs | Normal development |
| `cargo check` | Checks for errors without building | Fast error-check while editing |

`cargo check` is the one you'll use most while learning — it's much
faster than a full build because it skips generating machine code.

## Change it and re-run

Edit `src/main.rs`:

```rust
fn main() {
    println!("Hello, world!");
    println!("Rust is watching my memory.");
}
```

Run `cargo run` again.

**Check:** both lines print. Notice Cargo only recompiled what
changed — the second run is faster.

## Where the binary went

After `cargo build`, the compiled program lives at
`target/debug/hello` (or `hello.exe` on Windows). You can run it
directly:

```sh
./target/debug/hello
```

`cargo run` is just a shortcut for build-then-run-this-file.

## A first taste of the compiler watching you

Try this (it will fail — that's the lesson):

```rust
fn main() {
    let x = 5;
    println!("x is {}", x);
    x = 6;
    println!("x is now {}", x);
}
```

Run `cargo check`.

**Check:** the compiler refuses, with an error about "cannot assign
twice to immutable variable." In Rust, variables are **immutable by
default** — you can't change them unless you ask for mutability with
`let mut`. This feels annoying on day one and wonderful by month
two: an entire class of "wait, who changed this?" bugs can't happen.

The fix, for the curious:

```rust
fn main() {
    let mut x = 5;
    println!("x is {}", x);
    x = 6;
    println!("x is now {}", x);
}
```

## Check

- [ ] `cargo new`, `cargo run`, `cargo check` all work
- [ ] You can say where the compiled binary lives
- [ ] You can explain why `let x = 5; x = 6;` fails

Next: [Variables and Types](variables-and-types.md).
