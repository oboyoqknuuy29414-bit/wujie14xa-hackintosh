# 安装大致过程

> 本流程仅基于一台同型号机器的实测，不保证适用于所有机器，请以实际为准。

## 1. 前置

- macOS Tahoe **26.6** 安装镜像（dmg / iso）
- **Windows环境下使用etcher刻录启动U盘，也可以使用RDR恢复方式安装
- 使用本项目的EFI引导
tips:win下可以使用diskgenies把win放入efi目录下

## 2. 引导

开机按时选 U 盘 → OpenCore → 选安装盘

## 3. 安装

- 磁盘工具里先把目标分区抹成 **APFS**
- 安装到该分区，约 20～40 分钟

## 4. 装完第一次启动

- 请确保EFI的config中开启了 -s（单用户模式），没有请在boot-args下添加-s和-v参数（本项目EFI默认不开启）
- 禁用EFI中的“SMCBatteryManager.kext”否则但用户界面下会持续输出电量读取，无法输入指令
- 看到跑码停住，不重启，并且能输入指令时为引导成功

## 5.安装nullarc

- 1.确保EFI中包含“PhxBringup.kext”
- 2.在启动U盘的界面打开终端（如果使用RDR恢复镜像安装，请检查是否有rec分区，在rec模式下打开终端）
- 3.使用终端将nullarc0.4.0-alpha.zip移动至/Users/Shared目录下（目录和物理路径因人而异，不会的自主AI）
- 4.输入reboot 引导到你安装系统的磁盘
- 5.解压zip文件，并cd到目录下，输入bash ./install.sh --check 误报错输入bash ./install --windowserver进行安装，结束后重启两次
## 常见问题纠错

| 现象 | 原因 | 处理 |
|---|---|---|
| `still waiting for root device` | USB 掉盘 | 确认 `XHCILagFix.kext` 已启用 |
| 黑屏只有光标 | 无 GPU 驱动 | 跑 `install.sh --windowserver` 并重启两次 |
| 灰禁符 + 自动重启 | booter 拒绝 | 可能也和usb识别问题有关，请检查XHCILagFix.kext是否正确启用 |

nullarc驱动属于实验性，如果你发现其在某次重启后刷新率不正常，请重新安装并重启两次







