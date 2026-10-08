# syage_os

A hobby operating system written in Rust, following the [Writing an OS in Rust](https://os.phil-opp.com/) blog series by Philipp Oppermann.

## Status

- [x] Freestanding Rust binary (`#![no_std]`, `#![no_main]`)
- [x] VGA text buffer output
- [ ] Breakpoint exceptions
- [ ] Integrating a bootloader (`bootloader`)
- [ ] Hardware access through a Global Descriptor Table
- [ ] Catching interrupts with the Interrupt Descriptor Table
- [ ] Keyboard input
- [ ] Memory management
- [ ] Multitasking
- [ ] File system

## Prerequisites

- A Rust nightly toolchain with the `rust-src` and `llvm-tools-preview` components
- [`bootimage`](https://github.com/rust-osdev/bootimage) to build and run bootable disk images
- [`QEMU`](https://www.qemu.org/) to run the OS in a virtual machine

```sh
rustup override set nightly
rustup component add rust-src llvm-tools-preview
cargo install bootimage
```

## Building and running

```sh
cargo build
cargo run
```

`cargo run` is wired up through `.cargo/config.toml` to invoke `bootimage runner`,
which builds a bootable image and launches it in QEMU.

## Project layout

- `src/main.rs` — kernel entry point (`_start`), VGA output, and panic handler
- `Cargo.toml` — package manifest; `panic = "abort"` for both dev and release profiles
- `x86_64-syage_os.json` — custom bare-metal target specification
- `.cargo/config.toml` — target selection, `build-std` settings, and the QEMU runner

## License

See the repository for license details.

## References

- [Writing an OS in Rust](https://os.phil-opp.com/)
- [The `bootloader` crate](https://github.com/rust-osdev/bootloader)
