# WUJIE 14XA · macOS Tahoe 26.6 黑苹果

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

1.使用etcher在win下制作启动U盘，并将该EFI放入U盘的启动分区

2.在启动U盘中将macos安装到你的磁盘上

3.在启动U盘（如果你使用RDR恢复镜像法则可以使用硬盘上的rec分区）

4.nullarc安装过程参考其官方文档
## 功能状态

| 功能 | 状态 |
|---|---|
| 显卡加速 Metal | ✅ |
| 电池百分比 |   | ✅ |
|休眠/唤醒| |⚠️ 可以休眠及唤醒，单休眠时背光无法关闭|
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

1.编辑config可以使用OCAT项目在win下编辑

2.请在boot-args下添加-s和-v以进入单用户模式和开启跑码，初次启动请用“SMCBatteryManager.kext”
否则在单用户模式下会导致持续性跑码无法输入命令

3.请随机生成三码，请勿使用EFI自带的三码
## 文档

`docs/INSTALL.md`、`docs/POST-INSTALL.md`、`docs/TROUBLESHOOTING.md`、`THIRD-PARTY.md`

## 致谢

[nullarc](https://github.com/luuumi-dev/nullarc)（780M 驱动）、[XHCILagFix](https://github.com/hbyonly1/XHCILagFix)（USB 延迟）。

**作者**：[@oboyoqknuuy29414-bit](https://github.com/oboyoqknuuy29414-bit)（实机测试）· **DeepSeek（AI 助手）**（配置整理、文档、排障）

## 许可

文档 MIT；EFI 内第三方组件遵循各自许可。不含 Apple 软件，仅供个人学习研究。
