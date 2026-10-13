# syage_os

A hobby operating system written in Rust, following the [Writing an OS in Rust](https://os.phil-opp.com/) blog series by Philipp Oppermann.

## Status

- [x] Freestanding Rust binary (`#![no_std]`, `#![no_main]`)
- [x] VGA text buffer output via a `vga_buffer` module with `println!`/`print!` macros
- [x] Panic messages printed to the VGA buffer
- [x] Unit and integration testing with a custom test framework and serial output
- [x] Breakpoint exceptions
- [x] Catching interrupts with the Interrupt Descriptor Table
- [ ] Integrating a bootloader (`bootloader`)
- [ ] Hardware access through a Global Descriptor Table
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

## Testing

```sh
cargo test
```

Tests run inside QEMU. The kernel uses Rust's `custom_test_frameworks` feature to
define a `#[test_case]` attribute, and reports results over the serial port. QEMU
is configured with the `isa-debug-exit` device so `bootimage` can map the kernel's
exit code to a success/failure status for `cargo test`.

- `src/lib.rs` — the `Testable` trait, `test_runner`, `test_panic_handler`, and `exit_qemu`
- `src/serial.rs` — serial port driver plus the `serial_print!`/`serial_println!` macros
- `src/interrupts.rs` — IDT setup and the breakpoint exception test case
- `tests/basic_boot.rs` — integration test with its own `_start` entry point
- `tests/should_panic.rs` — test that verifies a panic occurs (no test harness)

## Project layout

- `src/main.rs` — kernel entry point (`_start`) and panic handler
- `src/lib.rs` — reusable kernel library, `init` routine, and test framework, shared by all test executables
- `src/interrupts.rs` — Interrupt Descriptor Table with a breakpoint exception handler
- `src/vga_buffer.rs` — safe interface to the VGA text buffer, plus the `println!`/`print!` macros
- `src/serial.rs` — 16550 UART serial driver used to report test results to the host
- `tests/` — integration tests, each compiled as a separate bare-metal executable
- `Cargo.toml` — package manifest; uses `lazy_static`, `spin`, `volatile`, and `uart_16550`
- `x86_64-syage_os.json` — custom bare-metal target specification
- `.cargo/config.toml` — target selection, `build-std` settings, and the QEMU runner

## License

See the repository for license details.

## References

- [Writing an OS in Rust](https://os.phil-opp.com/)
- [Testing](https://os.phil-opp.com/testing/)
- [The `bootloader` crate](https://github.com/rust-osdev/bootloader)
- [The `uart_16550` crate](https://github.com/rust-osdev/uart_16550)
