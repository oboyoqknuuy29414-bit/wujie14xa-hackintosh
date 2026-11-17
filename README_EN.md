# WUJIE 14XA · macOS Tahoe 26.6 Hackintosh

[中文](README.md) · English

MECHREVO WUJIE 14XA (Ryzen 7 H 255 + Radeon 780M) — OpenCore EFI and install notes.

> ⚠️ One-machine record; not guaranteed for every unit of this model — verify yourself.

> Status: installable, light daily use. GPU / battery / sleep / USB OK; trackpad, Wi-Fi, audio missing. GPU via nullarc (alpha).

## Hardware

- Ryzen 7 H 255 (Zen4) / Radeon 780M (1002:1900)
- 32GB LPDDR5 · 14" 2880×1800 120Hz
- PS/2 keyboard · I2C HID trackpad
- Wi-Fi MT7922 / RZ616 (14c3:0616)

## Install

1. Make the USB installer with etcher on Windows; copy `EFI/` to the USB EFI partition.
2. Boot it and install macOS to your disk.
3. Boot USB again (or on-disk Recovery after an RDR restore).
4. Install nullarc per its docs.

## Status

| Feature | State |
|---|---|
| Metal GPU | ✅ |
| Battery % | ✅ |
| Sleep/wake | ⚠️ works, backlight stays on |
| USB | ✅ |
| PS/2 keyboard | ✅ |
| Charging state | ❌ needs SSDT-ADP1 |
| I2C trackpad | ❌ VoodooI2C hangs |
| Audio | ❌ needs OCLP |
| External display | ❌ |
| Wi-Fi/BT | ⚠️ experimental |

## Wi-Fi (experimental)

No official MT7922 driver. Two unverified routes: `itlmt7922` (Ethernet-style, no AirDrop) or Sunrise native (`AppleSunriseWLAN.dext` + firmware + ID spoof; needs SIP/authenticated-root off). No MediaTek driver shipped here.

## First boot

1. Edit `config.plist` (OCAT on Windows).
2. Add `-s -v` to boot-args; disable `SMCBatteryManager.kext` on first boot or ACPI spam blocks the console.
3. Generate your own serials (GenSMBIOS); don't use the bundled ones.

## Docs · Credits · License

Docs: `docs/INSTALL.md`, `POST-INSTALL.md`, `TROUBLESHOOTING.md`, `THIRD-PARTY.md`

Credits: nullarc (780M driver) · XHCILagFix (USB latency). Authors: @oboyoqknuuy29414-bit (testing) · DeepSeek (AI assistant).

License: docs MIT; EFI binaries follow upstream; no Apple software.
