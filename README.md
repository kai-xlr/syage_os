# syage_os

A hobby operating system written in Rust, following the [Writing an OS in Rust](https://os.phil-opp.com/) blog series by Philipp Oppermann.

## Status

- [x] Freestanding Rust binary (`#![no_std]`, `#![no_main]`)
- [ ] VGA text buffer output
- [ ] Breakpoint exceptions
- [ ] Integrating a bootloader (`bootloader`)
- [ ] Hardware access through a Global Descriptor Table
- [ ] Catching interrupts with the Interrupt Descriptor Table
- [ ] Keyboard input
- [ ] Memory management
- [ ] Multitasking
- [ ] File system

## Prerequisites

- A Rust nightly toolchain (the blog posts use nightly features such as `custom_test_frameworks`)
- [`cargo-xbuild`](https://github.com/rust-osdev/cargo-xbuild) or a nightly with `-Zbuild-std`, depending on the chapter
- [`QEMU`](https://www.qemu.org/) to run the OS in a virtual machine

```sh
rustup override set nightly
```

## Building and running

```sh
cargo build
cargo run
```

## Project layout

- `src/main.rs` — kernel entry point (`_start`) and panic handler
- `Cargo.toml` — package manifest; `panic = "abort"` for both dev and release profiles

## License

See the repository for license details.

## References

- [Writing an OS in Rust](https://os.phil-opp.com/)
- [The `bootloader` crate](https://github.com/rust-osdev/bootloader)
