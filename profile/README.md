# Embedded Rust with stm32 and Embassy

Video that explains most of the setup [Embedded Rust setup explained](https://www.youtube.com/watch?v=TOAynddiu5M)

## Windows build tools
[Visual Studio C++ Build tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
- MSVC v143 - VS 2022 C++ x64/x86 build tools (latest)
- Windows 11 SDK

## Rust setup
[Install Rust](https://www.rust-lang.org/tools/install)

## ST-link
[ST-link sample](https://github.com/WeActStudio/WeActStudio.MiniDebugger)

[Driver for Windows](https://www.st.com/en/development-tools/stsw-link009.html)

[Firmware update](https://www.st.com/en/development-tools/stsw-link007.html)

## Flash and debug
[probe-rs](https://probe.rs/docs/getting-started/installation/)

Usefull commands:
```powershell
cargo install probe-rs-tools --git https://github.com/probe-rs/probe-rs
probe-rs chip list | select-string stm32f411ce

rustup update
rustup target add thumbv7em-none-eabihf
rustup show

rustup component add llvm-tools
cargo install cargo-binutils
```

### Trouble flashing target:
```
 WARN probe_rs::probe::stlink: send_jtag_command 242 failed: SwdDpWait
 WARN probe_rs::probe::stlink: got SwdDpWait/SwdApWait, retrying.
 WARN probe_rs::probe::stlink: send_jtag_command 242 failed: SwdDpWait
 WARN probe_rs::probe::stlink: got SwdDpWait/SwdApWait, retrying.
```
- Press and hold **BOOT0**
- Press and release **RESET**
- Release **BOOT0**

If using the WeAct mini debugger, connect NRST to R on stm32

## VS Code

Recommended extensions:
- rust-analyzer
- Even Better TOML
- Dependi
- Error Lens
- Debugger for probe-rs
