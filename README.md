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

## 安装（Windows + Etcher，不需要 Mac）

1. [balenaEtcher](https://etcher.balena.io/) → `Flash from file` 选 macOS 26.6 镜像 → 16GB+ U 盘 → Flash
2. 把本仓库 `EFI/` 覆盖到 U 盘 EFI 分区（`diskpart` 分配盘符，或 DiskGenius）
3. BIOS：Secure Boot 关、CSM 关、Above4G 开、ReBAR 关、**IOMMU 关**、UMA 1G
4. U 盘启动 → 安装到 NVMe
5. 装完在**图形终端**（非单用户）执行：

```
cd ~/Downloads/nullarc-0.4.0-alpha
sudo ./install.sh --check
sudo ./install.sh --windowserver
sudo reboot
```

必须 `sudo`、必须真重启；重启两次后 WindowServer 才走 GPU。

> **USB 可直接装，不需要 RDR 镜像**：EFI 内置 `XHCILagFix`，修掉 AMD 平台 ~50ms 的 USB 完成事件延迟。

## 功能状态

| 功能 | 状态 |
|---|---|
| 显卡加速 Metal | ✅ |
| 电池百分比 / 休眠 | ✅ |
| USB（含 U 盘） | ✅ |
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

config 里序列号是占位值，用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 生成 `MacBookPro16,1` 的替换。**占位号别登 iCloud/App Store。**

## 文档

`docs/INSTALL.md`、`docs/POST-INSTALL.md`、`docs/TROUBLESHOOTING.md`、`THIRD-PARTY.md`

## 致谢

[nullarc](https://github.com/luuumi-dev/nullarc)（780M 驱动）、[XHCILagFix](https://github.com/hbyonly1/XHCILagFix)（USB 延迟）。

**作者**：[@oboyoqknuuy29414-bit](https://github.com/oboyoqknuuy29414-bit)（实机测试）· **DeepSeek（AI 助手）**（配置整理、文档、排障）

## 许可

文档 MIT；EFI 内第三方组件遵循各自许可。不含 Apple 软件，仅供个人学习研究。
