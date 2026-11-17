# 无界 14XA · macOS Tahoe 26.6 黑苹果

机械革命 **WUJIE 14XA**（AMD Ryzen 7 H 255 + Radeon 780M）的 OpenCore 配置与安装记录。

> 状态：可安装、可日用（尝鲜级）。显卡加速/电池/休眠/USB 正常；触控板、Wi-Fi、声卡仍缺。
> 图形驱动依赖 [nullarc](https://github.com/luuumi-dev/nullarc)（alpha），非原生支持。

## 硬件

| 部件 | 规格 |
|---|---|
| 机型 | 无界 14XA（14"） |
| CPU | AMD Ryzen 7 H 255（Zen4 8C/16T） |
| iGPU | Radeon 780M（RDNA3，1002:1900） |
| 内存 | 32GB LPDDR5 |
| 屏幕 | 2880×1800 120Hz eDP |
| 键盘/触控板 | PS/2 键盘 + I2C HID 触控板 |
| 无线 | MT7922 / RZ616（14c3:0616，焊死） |

## 关键点：USB 可以直接装，不需要 RDR 镜像

本 EFI 内置 **XHCILagFix**。AMD 平台 USB 完成事件中断平均延迟约 50ms，
表现为安装器读盘卡死、U 盘不识别、鼠标卡顿。加上这个补丁后 USB 恢复正常，
**可以直接用 createinstallmedia 做的 U 盘安装系统**。

## 安装

1. Mac 上 `createinstallmedia` 做 Tahoe 26.6 安装盘，放入本仓库 `EFI/`
2. BIOS：Secure Boot 关、CSM 关、Above4G 开、ReBAR 关、**IOMMU 关**、UMA 1G
3. U 盘启动安装
4. 装完在 **图形界面终端**里（不要单用户）：
   `sudo ./install.sh --check` → `sudo ./install.sh --windowserver` → `sudo reboot`
5. 重启两次后 WindowServer 才走 GPU 合成

详细步骤见包内 `docs/`。

## 功能状态

| 功能 | 状态 |
|---|---|
| 显卡加速 Metal | ✅ |
| 电池百分比 | ✅ |
| 休眠 | ✅ |
| USB（含 U 盘） | ✅（XHCILagFix） |
| 键盘 PS/2 | ✅ |
| 触控板 I2C | ❌ VoodooI2C 在 AMD 上卡 PCI |
| 声卡 | ❌ 需 OCLP 恢复 AppleHDA |
| Wi-Fi / 蓝牙 | ❌ MT7922 无驱动（焊死不可换） |
| 外接显示器 | ❌ nullarc 未支持 |

## 首次使用

config 里序列号是占位值，请用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 生成 `MacBookPro16,1` 的序列号替换。
**占位序列号不要登录 iCloud / App Store。**

## 下载

完整包（EFI + 文档 + 说明）见 [Releases](../../releases)。

## 致谢

- [nullarc](https://github.com/luuumi-dev/nullarc) — Radeon 780M 驱动
- [XHCILagFix](https://github.com/hbyonly1/XHCILagFix) — AMD USB 延迟修复
- [OpenCore](https://github.com/acidanthera/OpenCorePkg) 及 Acidanthera 组件

## 许可

文档 MIT；EFI 内第三方组件遵循各自许可，详见包内 THIRD-PARTY.md。
本项目不含任何 Apple 软件，仅供个人学习研究。
