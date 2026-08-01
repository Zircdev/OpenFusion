<div align="center">
    <img src="/Logos.png" alt="OpenFusionLogos" width="64"/>
</div>

<h1 align="center">OpenFusion</h1>

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

<div style="display: flex; justify-content: center;">
  <img src="https://readme-typing-svg.demolab.com?font=Noto+Sans+SC&weight=600&size=22&pause=1000&color=FF0000&width=800&lines=OpenFusion%EF%BC%9A%E4%B8%BA%E5%85%BC%E5%AE%B9%E8%80%8C%E7%94%9F;Fusion+%F0%9F%90%82%F0%9F%8D%BA;99.99%25;%E7%89%9B%E9%80%BC%E6%98%AF%E4%BA%BA%E7%B1%BB%E8%BF%9B%E6%AD%A5%E7%9A%84%E7%9B%B4%E5%8D%87%E9%A3%9E%E6%9C%BA;%E6%8B%BF%E6%9D%A5%E5%90%A7%E4%BD%A0%EF%BC%81" width="100%" />
</div>

## 简介

OpenFusion 致力于高性能兼容一切操作系统的程序，我们目前兼容且可以执行 Linux 的 ELF 程序，支持 AMD、NVIDIA、INTEL、VMWARE 的核显驱动，理论帧率最高可达 534 FPS 。

当前版本：**IS-B607.120647X**

## 特性

- **图形界面**：完整的 GUI 桌面环境（任务栏、开始菜单、窗口管理、鼠标/滚轮支持）
- **内置应用**（写死在代码里的，放心吧删不掉）：终端（Shell）、文件资源管理器、记事本、计算器、Velox 3D、WebView 浏览器、图片查看器、MP3 播放器
- **Wei 语言**：系统级 API 与 Wei 编程语言深度绑定，你可以肥肠卿松的 Wei 编写 OpenFusion 应用程序
- **文件系统**：自研 GFS（Geo File System），同时我们也兼容一些垃圾文件系统 NTFS、FAT32
- **WebView**：内核内置嵌入式浏览器，支持 HTML/CSS/JS 渲染
- **Linux 兼容**：可执行 Linux ELF 程序（musl 工具链）
- **驱动**：AMD、NVIDIA、INTEL、VMWARE 核显驱动

## 快速开始

##### 1. 安装工具链

需要 **x86_64 交叉编译器**，不能使用系统默认编译器：

- `x86_64-w64-mingw32-gcc`（编译 bootx64.c）
- `x86_64-elf-g++`（编译内核）
- `x86_64-elf-ld`（链接）
- QEMU（可选，用于虚拟机测试，如果你是猛新后边会教你怎么直接裸机启动）

**Windows 用户**：从 [Releases 页面](https://github.com/Zircdev/OpenFusion/releases) 下载交叉工具链，解压后将 bin 目录加入 PATH。

**Linux 用户**：

```bash
sudo apt install gcc-x86-64-linux-gnu nasm make qemu-system-x86 mtools
```

##### 2. 构建

```bash
cd OpenFusion
.\build.bat          # 构建
.\build.bat run      # 构建并启动 QEMU
```

或使用 Makefile：

```bash
make all
make run
```

##### 3. 在 QEMU 中运行

`build.bat run` 会自动启动 QEMU（需要安装 QEMU 到 `C:\Program Files\qemu\`）。也可手动运行：

```bash
qemu-system-x86_64 -machine q35,accel=tcg -cpu qemu64 -smp 2 -m 1024 -bios OVMF.fd -drive file=fat:rw:esp,format=vvfat -device qemu-xhci -device usb-tablet -device usb-kbd -serial file:serial.log
```

##### 4. 可选：写入真实硬件

构建完成后，将 `esp/` 目录内容拷贝到 U 盘的 EFI 分区，从 UEFI 模式启动即可。

如果不会的话或者有什么问题，打开 Cluade Code 一顿猛戳绝对能成功！ 😊

## 项目结构

```
OpenFusion/
├── bootx64.c              # UEFI 引导程序
├── build.bat              # Windows 构建脚本
├── Makefile               # make 构建系统
├── kernel/                # 内核源码
│   ├── main.cpp           # 主内核 + GUI 桌面环境
│   ├── core/              # 系统 API（窗口、SDF）
│   ├── gui/               # GUI 绘制（Aero、合成器、TTF 字体）
│   ├── apps/              # 内置应用（计算器、记事本、WebView）
│   ├── wei/               # Wei 语言解释器
│   ├── velox/             # Velox 3D 引擎
│   ├── linux/             # Linux 兼容层（musl ELF）
│   ├── net/               # 网络（socket）
│   └── drivers/           # 磁盘驱动（FAT32、NTFS、GFS）
├── include/               # 公共头文件
├── resources/             # 系统资源（图标、字体）
├── osfile/                # 用户文件系统内容
├── esp/                   # ESP 部署目录（构建产物）
├── settings.h             # 屏幕分辨率设置
└── temp_opencode/         # 开发缓存（可忽略）
```

## 命令行

在内置终端中输入 `help` 查看完整命令列表，常用命令：

```
ls / cd / pwd        文件系统
cat <file>           查看文件
.\file               打开文件（按类型启动对应应用）
\prog [args]         执行程序（支持 ELF 可执行文件）
wei <script>         运行 Wei 语言脚本
startgui             启动 GUI 桌面
neofetch             系统信息
smooth               切换字体抗锯齿
```

至于其他命令······ 你怎么玩 MC 你就怎么试吧，跟 Linux 的命令一点也不像没想到吧！！！ 😄

## 下载

预构建的镜像文件和交叉工具链可在 Releases 页面获取：

**[Releases 下载页](https://github.com/Zircdev/OpenFusion/releases)**

| 文件                       | 说明                                                     |
| :------------------------- | :------------------------------------------------------- |
| `OpenFusion-*.iso`         | CD/DVD 镜像，适用于虚拟机或光盘启动                      |
| `OpenFusion-*.img`         | 硬盘镜像，可写入 U 盘（`dd` 或 Rufus）                   |
| `cross-toolchain-*.tar.gz` | Windows 交叉工具链（MinGW-w64），**仅 Windows 用户需要** |

## 贡献者

感谢所有为 OpenFusion 贡献代码的朋友！ 🎉

<a href="https://github.com/Zircdev/OpenFusion/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Zircdev/OpenFusion" />
</a>

[贡献者](https://github.com/Zircdev/OpenFusion/graphs/contributors)

## 开源协议

该程序在 [Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0.html) 开源协议下发布。

## 作者的话 😄

\u0046\u0075\u0073\u0069\u006F\u006E\u725B\u903C\u0021\u0021\u0021
