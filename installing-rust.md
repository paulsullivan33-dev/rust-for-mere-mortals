# Installing Rust

**Scope:** getting the Rust toolchain on your machine and proving it
works. The toolchain is `rustc` (the compiler), `cargo` (build tool
and package manager), and `rustup` (the installer/updater).

## The short version

On all three major OSes, the recommended install is **rustup**, a
small program that installs and updates the Rust toolchain.

## Windows

1. Download `rustup-init.exe` from
   [rustup.rs](https://rustup.rs) (or
   [the Rust site](https://www.rust-lang.org/tools/install)).
2. Run it. Accept the defaults (press Enter at each prompt).
3. Close and reopen your terminal (PowerShell or Command Prompt).

**Check:**

```powershell
rustc --version
cargo --version
```

You should see version numbers, e.g. `rustc 1.8x.x`.

> Note: the installer may ask you to install the Visual Studio C++
> build tools first. That's normal — Rust uses Microsoft's linker on
> Windows. The installer links you to them.

## Mac

In Terminal:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Accept the defaults, then restart your terminal (or run the
`source` command the installer prints).

**Check:**

```sh
rustc --version
cargo --version
```

## Linux

Same command as Mac, in your terminal:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Accept the defaults, restart your terminal (or run the printed
`source` command).

**Check:**

```sh
rustc --version
cargo --version
```

> Your distribution may also offer Rust via its package manager
> (`apt install rustc`, etc.). Those versions are often months old.
> rustup stays current — prefer it.

## The three tools, plainly

| Tool | Job | You'll run it... |
|---|---|---|
| `rustup` | Installs/updates Rust itself | Rarely — `rustup update` every so often |
| `rustc` | Compiles one file directly | Rarely — Cargo calls it for you |
| `cargo` | Builds projects, runs them, manages libraries | Constantly — this is your main tool |

## Keeping it updated

```sh
rustup update
```

Run that every few weeks. Rust releases a new version every six
weeks; updates are painless.

## Check

- [ ] `rustc --version` prints a version number
- [ ] `cargo --version` prints a version number
- [ ] `rustup update` runs without errors

Next: [Your First Program](first-program.md).
