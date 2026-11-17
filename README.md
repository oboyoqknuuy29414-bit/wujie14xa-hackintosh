# WUJIE 14XA · macOS Tahoe 26.6 黑苹果

[English](README_EN.md) · 中文

机械革命 **无界 14XA**（AMD Ryzen 7 H 255 + Radeon 780M）OpenCore 配置与安装记录。

> ⚠️ 本仓库仅为一台同型号机器的实测记录，不保证适用于所有同型号机器，请以实际为准。

> 状态：可安装、可日用（尝鲜级）。显卡加速/电池/休眠/USB 正常；触控板、Wi-Fi、声卡仍缺。图形驱动依赖 [nullarc](https://github.com/luuumi-dev/nullarc)（alpha）。

## 硬件

- CPU：AMD Ryzen 7 H 255（Zen4）
- iGPU：Radeon 780M（RDNA3，`1002:1900`）
- 32GB LPDDR5 ｜ 14" 2880×1800 120Hz eDP
- PS/2 键盘 ｜ I2C HID 触控板
- 无线：MT7922 / RZ616（`14c3:0616`）

## 安装

1. 使用 etcher 在 win 下制作启动 U 盘，并将该 EFI 放入 U 盘的启动分区
2. 在启动 U 盘中将 macOS 安装到你的磁盘上
3. 在启动 U 盘（如果你使用 RDR 恢复镜像法，则可以使用硬盘上的 rec 分区）
4. nullarc 安装过程参考其官方文档

## 功能状态

| 功能 | 状态 |
|---|---|
| 显卡加速 Metal | ✅ |
| 电池百分比 | ✅ |
| 休眠/唤醒 | ⚠️ 可以休眠及唤醒，但休眠时背光无法关闭 |
| USB | ✅ |
| 键盘 PS/2 | ✅ |
| 充电状态 | ❌ 待 SSDT-ADP1 |
| 触控板 I2C | ❌ VoodooI2C 卡 PCI |
| 声卡 | ❌ 需 OCLP |
| 外接显示器 | ❌ |
| Wi-Fi / 蓝牙 | ⚠️ 实验性 |

## 无线：实验性（experimental）

MT7922 无官方驱动。社区两条**实验性**路线，均未验证稳定：

- `itlmt7922`（第三方 kext）：以太网式接口 + 菜单栏 App，无原生菜单/AirDrop
- Sunrise 原生路线：复用 26 内置 `AppleSunriseWLAN.dext` + 固件 + device-id 伪装，需关 SIP/authenticated-root 并建系统卷快照

本仓库**不含**任何 MediaTek 驱动，自行评估风险。

## 首次使用

1. 编辑 config 可以使用 OCAT 项目在 win 下编辑
2. 请在 boot-args 下添加 `-s` 和 `-v` 以进入单用户模式和开启跑码；初次启动请先禁用 `SMCBatteryManager.kext`，否则单用户模式下会持续跑码、无法输入命令
3. 请随机生成三码，请勿使用 EFI 自带的三码

## 文档

`docs/INSTALL.md`、`docs/POST-INSTALL.md`、`docs/TROUBLESHOOTING.md`、`THIRD-PARTY.md`

## 致谢

[nullarc](https://github.com/luuumi-dev/nullarc)（780M 驱动）、[XHCILagFix](https://github.com/hbyonly1/XHCILagFix)（USB 延迟）。

**作者**：[@oboyoqknuuy29414-bit](https://github.com/oboyoqknuuy29414-bit)（实机测试）· **DeepSeek（AI 助手）**（配置整理、文档、排障）

## 许可

文档 MIT；EFI 内第三方组件遵循各自许可。不含 Apple 软件，仅供个人学习研究。
