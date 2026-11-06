# Windows 8.1 x64 VLC edition

This source tree has a dedicated Windows build configuration. On Windows, the
player detector exposes VLC only; MPV and IINA are not offered. Existing
catalogue, provider, search, download, favorites, history, addon, subtitle,
and playback code remains in place.

## Runtime requirements

- 64-bit Windows 8.1.
- 64-bit VLC installed separately.
- A terminal that supports the application's TUI (Windows Terminal is not
  available on Windows 8.1; use a compatible console host such as the classic
  console or ConEmu).

If VLC is installed outside its standard location, set the environment
variable `MOVIEBOX_VLC_PATH` to the full path of `vlc.exe`.

## Build

The normal `x86_64-pc-windows-msvc` Rust target requires Windows 10 or newer.
This edition instead targets `x86_64-win7-windows-msvc`, which has a Windows 7
baseline and therefore a suitable OS baseline for Windows 8.1. It is a Tier 3
Rust target and Rust does not distribute its standard library prebuilt for it;
the standard library must be built from source.

On a Windows build runner with the Visual Studio C++ toolchain installed:

```powershell
rustup toolchain install nightly --component rust-src
cargo +nightly build -Z build-std=std,panic_abort --target x86_64-win7-windows-msvc --release --locked
```

The binary is written to:

```text
target\x86_64-win7-windows-msvc\release\moviebox-tui.exe
```

A GitHub Actions workflow is included at `.github/workflows/windows-8.1.yml`.
Run it using the Actions tab's **Windows 8.1 x64 VLC build** workflow and
 download the resulting artifact.

## Compatibility warning

The build workflow checks that the executable starts and prints its version on
the Windows build runner. That is not a full Windows 8.1 playback test. The
actual executable must still be tested on a Windows 8.1 machine with VLC
installed. Provider availability and stream compatibility depend on external
services and can change independently of this application.
