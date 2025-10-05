# Embedded Rust with STM32 and Embassy

Video that explains most of the setup [Embedded Rust setup explained](https://www.youtube.com/watch?v=TOAynddiu5M)

## Rust Install

### Windows Build Tools
Install required components with [Visual Studio C++ Build tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) or [Visual Studio Installer](https://visualstudio.microsoft.com/downloads/).
- MSVC v143 - VS 2022 C++ x64/x86 build tools (latest)
- Windows 11 SDK (10.0.xxxxx.x)

### Install Rust with `rustup`
[Install Rust](https://www.rust-lang.org/tools/install). Minimal profile may be selected.

## Flash and Debug
[probe-rs](https://probe.rs/docs/getting-started/installation/) is a library and CLI tool for flashing, debugging, and interacting with embedded devices via debug probes (like ST-Link, J-Link).

Install probe-rs release version:
```powershell
cargo install probe-rs-tools --force
```

Or, install probe-rs development version:
```powershell
cargo install probe-rs-tools --git https://github.com/probe-rs/probe-rs --force
```

### ST-link for flash and debug
[WeAct Minidebugger](https://github.com/WeActStudio/WeActStudio.MiniDebugger) may be used for flash and debug.
- ST-link [Driver for Windows](https://www.st.com/en/development-tools/stsw-link009.html).
- ST-link [Firmware Update Tool](https://www.st.com/en/development-tools/stsw-link007.html).

### STM32 Chip Type 
The correct `target` for your STM32 chip must be added with `rustup`. 

Target name is in `arch-vendor-os-abi` format. STM32F411CE is `thumbv7em-none-eabihf`:
- thumbv7em: ARMv7E-M architecture (e.g., Cortex-M4/M7)
- none: No OS (bare metal)
- eabihf: Embedded ABI, hard float (FPU with hardware support)

Chip information [probe-rs targets](https://probe.rs/targets/?manufacturer=STMicroelectronics&family=SHOW_ALL_FAMILIES).

Add target:
```
rustup update
rustup target list
rustup target list --installed
rustup target add thumbv7em-none-eabihf
rustup target add thumbv6m-none-eabi
rustup target add thumbv7em-none-eabi
rustup target add thumbv8m.main-none-eabihf
rustup target add thumbv7m-none-eabi
rustup show
# rustup uninstall <target, toolchain...>

probe-rs chip list | select-string stm32f411ce
```

Usefull tools:
```
rustup component add rustfmt
rustup component add clippy
rustup component add llvm-tools
cargo install cargo-binutils

cargo install --list
# cargo update
# cargo uninstall <package>
```

### Troubleshooting:
When trying to flash and run with `cargo run`:

```
 WARN probe_rs::probe::stlink: send_jtag_command 242 failed: SwdDpWait
 WARN probe_rs::probe::stlink: got SwdDpWait/SwdApWait, retrying.
 WARN probe_rs::probe::stlink: send_jtag_command 242 failed: SwdDpWait
 WARN probe_rs::probe::stlink: got SwdDpWait/SwdApWait, retrying.
```
#### Manual fix
- Press and hold **BOOT0**
- Press and release **RESET**
- Release **BOOT0**

#### Automatic reset on flash
If using the WeAct Minidebugger, connect NRST to R/NR/NRST on STM32. The `--connect-under-reset` must be added. This may be done in `.cargo\config.toml`. Sample with `STM32F411CEUx`:
```toml
[build]
target = "thumbv7em-none-eabihf"

# [target.'cfg(all(target_arch = "arm", target_os = "none"))']
[target.thumbv7em-none-eabihf]
runner = "probe-rs run --chip STM32F411CEUx --connect-under-reset"
rustflags = ["-C", "link-arg=-Tlink.x", "-C", "link-arg=-Tdefmt.x"]

[env]
DEFMT_LOG = "trace"
```

## VS Code

Recommended extensions:
- Rust-Analyzer
- Even Better TOML
- Dependi
- Error Lens
- Debugger for probe-rs
