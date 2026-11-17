# 安装细节

> 本流程仅基于一台同型号机器的实测，不保证适用于所有机器，请以实际为准。

## 0. 前置

- macOS Tahoe **26.6** 安装镜像（dmg / iso）
- **Windows + [balenaEtcher](https://etcher.balena.io/)** 即可刻录，不需要 Mac
- 本仓库 `EFI/` 已含全部 kext，序列号是占位值
- BIOS 见 README 的表格

## 1. 刻录 + 放 EFI

1. Etcher → `Flash from file` 选镜像 → 选 U 盘（16GB+）→ Flash
2. 用 `diskpart` 给 EFI 分区分配盘符：

```
diskpart
list disk
select disk N
list partition
select partition 1
assign letter=Z
```

3. 把本仓库 `EFI/` 覆盖到 `Z:\EFI\`

`EFI/BOOT/BOOTx64.efi` 必须是 OpenCore 的那份（不是 Windows 的）。

## 2. 引导

开机按 F7（或 Del 启动菜单）选 U 盘 → OpenCore → 选安装盘。

> 第一次进安装器可能要等 1～3 分钟：GPU 在 bring-up，屏幕可能先黑后亮。

## 3. 安装

- 磁盘工具里先把目标分区抹成 **APFS**
- 安装到该分区，约 20～40 分钟

## 4. 装完第一次启动

- 还会进安装器第二阶段（选语言、装剩余部分），正常走完
- 进系统后按 [POST-INSTALL.md](POST-INSTALL.md) 装显卡驱动

## 常见坑

| 现象 | 原因 | 处理 |
|---|---|---|
| `still waiting for root device` | USB 掉盘 | 确认 `XHCILagFix.kext` 已启用 |
| 卡 `PCI configuration begin` | ACPI/PCI 枚举 | 检查是否误启用 VoodooI2C（默认关闭） |
| 黑屏只有光标 | 无 GPU 驱动 | 跑 `install.sh --windowserver` 并重启两次 |
| 灰禁符 + 自动重启 | booter 拒绝 | 检查 `SecureBootModel=Disabled`、序列号格式 |

## 双系统

一块盘、一个 ESP 即可：

```
EFI/BOOT/BOOTx64.efi             ← OpenCore（兜底）
EFI/OC/...                       ← 本仓库
EFI/Microsoft/Boot/bootmgfw.efi  ← Windows 安装时写入，别动
```
