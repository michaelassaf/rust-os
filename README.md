# hourglassOS

Hourglass OS strives to become the world's leading operating system for programmers to best with minimal setup and distraction free.

## Toolchain

This project uses the rust nightly compiler.

## Custom Target

Following [guide](https://os.phil-opp.com/minimal-rust-kernel/#target-specification) for initial target configuration.

Configration file: `x86_64-hourglass_os.json`

Disabled features due to performance impact.
This could be looked at later in depth to see how we can enable this feature.

`"features": "-mmx,-sse,+soft-float"`

## Building for bare metal

### Setup

To build our binary for our custom target, we need to recompile rust `core` and `compiler_builtins` libraries.
In order to recompile these libraries, cargo needs access to the rust source code. Execute the following.

`rustup component add rust-src`

### Build binary OS kernal

`cargo build`

_We don't need to pass `--target x86_64-hourglass_os.json` becayse we added this to our `config.toml` build configuration_

## Create a bootable image

### Setup

Install `bootimage` that handles compiling the `bootloader` and our `kernal`.

`cargo install bootimage`

This depends on the following component `llvm-tools-preview`

`rustup component add llvm-tools-preview`

### Create bootable image

The following crate will automatically combine the compiled `bootloader` and `kernal` into a bootable image.

`cargo bootimage`

After executing the command, you should see a bootable disk image named bootimage-hourglass_os.bin in your target/x86_64-hourglass_os/debug directory.

See more info [here](https://os.phil-opp.com/minimal-rust-kernel/#how-does-it-work) about what `bootimage` is doing behind the scenes.

## Run image in QEMU

### Setup

Install [QEMU](https://www.qemu.org/download/#macos)

### How to run

You can simply run `cargo run` which runs `bootimage runner` under the hood due to the `runner` `.cargo/config.toml` configuration.

or

try running it manually

`qemu-system-x86_64 -drive format=raw,file=target/x86_64-hourglass_os/debug/bootimage-hourglass_os.bin`
