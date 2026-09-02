# TechmaniaNfcHook

Windows x64 native CardIO bridge for TECHMANIA and the AIC Pico reader.

This project turns AIC Pico HID reports into a small, stable event ABI that a
Unity build can consume without blocking its main thread. It supports both
CardIO identities emitted by the firmware:

- FeliCa / amusement IC (`report 1`)
- MIFARE Ultralight / NTAG (`report 2`)

The plugin is a normal Unity native plugin loaded with `DllImport`. It is not
process injection, an IAT patch, or a replacement for a Windows system DLL.

## Quick start

1. Connect and configure AIC Pico. The CardIO HID interface must enumerate as
   VID `CAFF`, PID `400E`, usage page `FFCA`, usage `0001`.
2. Download `TechmaniaNfcHook.dll` and `TechmaniaNfcProbe.exe` from the
   [latest release](https://github.com/ryukikiyomizu/TechmaniaNfcHook/releases/latest).
3. Run `TechmaniaNfcProbe.exe --seconds 30`.
4. Copy `TechmaniaNfcHook.dll` to
   `TECHMANIA/Assets/Plugins/x86_64/TechmaniaNfcHook.dll`.
5. Add `unity/Runtime/TechmaniaNfcNative.cs` and
   `unity/Runtime/NfcReaderService.cs` to the game project.
6. Start and poll the service from the profile/session owner. Themes call only
   the reader-neutral `tm.profile` functions described in
   [Theme hooks](docs/THEME-HOOKS.md).

If the DLL or reader is absent, NFC disables itself. USB login and Guest play
remain available.

## Repository map

- `include/` - public C ABI
- `src/` - report decoder, state machine, HID worker, exported runtime
- `tools/` - redacted hardware probe
- `tests/` - deterministic native tests
- `unity/Runtime/` - safe managed P/Invoke boundary
- `unity/Tests/` - Unity contract tests and integration documentation
- GitHub Releases - ready-to-deploy x64 DLL, probe, and checksums
- `docs/` - architecture, ABI, build, theme, and operating guides

## Documents

- [Architecture](docs/ARCHITECTURE.md)
- [Native ABI](docs/ABI.md)
- [Building](docs/BUILDING.md)
- [Hardware setup](docs/HARDWARE-SETUP.md)
- [Unity integration](docs/UNITY-INTEGRATION.md)
- [Theme hooks](docs/THEME-HOOKS.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)

## Upstream hardware references

This bridge was developed against the public AIC Pico CardIO behavior and
documentation. The upstream hardware, firmware, and configuration utility are
maintained separately by their original author:

- [whowechina/aic_pico](https://github.com/whowechina/aic_pico)
- [AIC Pico Configurator](https://whowechina.github.io/aic_pico/Configurator/index.html)

No AIC Pico source code or firmware is vendored in this repository.

Author: [Ryuki](https://github.com/ryukiyomizuu). See [AUTHORS.md](AUTHORS.md).

