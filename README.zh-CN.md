# Awesome Run Anything Anywhere

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Link check](https://github.com/devinhurry/awesome-run-anything-anywhere/actions/workflows/links.yml/badge.svg)](https://github.com/devinhurry/awesome-run-anything-anywhere/actions/workflows/links.yml)

> 一份精心整理的项目地图：让软件运行在它原本并不支持的硬件或操作系统上。

[English](README.md) · **简体中文**

这份列表覆盖不同 CPU 架构、不同操作系统家族，以及 PC 与游戏主机之间的实用路线：API 兼容层、二进制翻译、全系统模拟、图形 API 翻译、虚拟机、浏览器沙箱、静态重编译与原生移植。

列表带有明确取舍：优先收录今天真正可用的项目，记录决定路线能否走通的限制，并把实验性项目清楚标注出来，而不是把它们包装成成品。第一版收录 **60 个项目**，已于 **2026-10-24** 对照一手资料核验。

如果你关心的是最受关注的路线——Apple silicon 上的 Windows 游戏、PC/Android/PS5 上的 Switch 1 游戏、PS5 Linux 与 PS5 RetroArch，或让 GBA 到处运行——请先看 [矩阵](#guest--host-矩阵) 与 [路线配方](#路线配方)。

## 目录

- [范围](#范围)
- [如何阅读](#如何阅读)
- [Guest → Host 矩阵](#guest--host-矩阵)
- [兼容层](#兼容层)
- [二进制翻译与用户态模拟](#二进制翻译与用户态模拟)
- [图形 API 翻译](#图形-api-翻译)
- [虚拟机与全系统模拟](#虚拟机与全系统模拟)
- [浏览器与 WebAssembly](#浏览器与-webassembly)
- [游戏主机与电脑模拟器](#游戏主机与电脑模拟器)
- [游戏引擎重实现](#游戏引擎重实现)
- [静态重编译与原生移植](#静态重编译与原生移植)
- [主机作为宿主](#主机作为宿主)
- [路线配方](#路线配方)
- [法律与实践说明](#法律与实践说明)
- [贡献](#贡献)

## 范围

**收录**

- 在一种宿主上运行 Windows、Linux、macOS、iOS、Android、DOS 与主机软件。
- x86、Arm、RISC-V、PowerPC 与 WebAssembly 之间的 CPU 翻译。
- 图形、音频、输入与操作系统服务的 API 翻译。
- 虚拟机、模拟器、静态重编译器，以及重建原始运行时的源码移植项目。
- 定义当前技术水平的商业与闭源工具。
- 具有历史意义或仍然不可替代的实验性、已停止项目。

**不收录**

- 普通应用容器、远程桌面与云串流服务。
- 商店、启动器与兼容性评分数据库。
- 去除 DRM、绕过反作弊与盗版工具。
- 仅分发游戏、固件、BIOS 或密钥的项目。

## 如何阅读

每个条目给出 Guest（被运行对象）、Host（宿主）、实现机制与主要限制。

| 标签 | 含义 |
| --- | --- |
| **成熟** | 在所述路线上被广泛使用且稳定；具体应用仍可能有兼容性差异。 |
| **活跃** | 仍在维护和改进，但兼容性或平台支持并不均衡。 |
| **实验性** | 早期阶段，支持的游戏或功能有限，预期会出错。 |
| **遗留** | 已停止、已弃用或被取代；仍可用于 Preservation 或对照。 |
| **专有** | 闭源或商业软件，可能提供试用版。 |

路线写作 **Guest → Host**。一条路线可以组合多个工具；例如 *Windows 应用 → Arm Linux* 通常意味着 Wine 加 FEX 或 Box64。矩阵列出最短的知名组合，而不是所有可能的堆栈。

## Guest → Host 矩阵

### 桌面、移动与浏览器宿主

出现项目名表示存在实用路线。*斜体* 表示实验性或受限路线。`—` 表示本列表中没有成熟路线。

| Guest | Windows | Linux | macOS | iOS / iPadOS | Android | 浏览器 |
| --- | --- | --- | --- | --- | --- | --- |
| Windows x86/x64 | 原生；Arm 上由 Prism 模拟 | Wine、Proton、CrossOver | CrossOver、Wine + Rosetta、GPTK | — | Winlator、Boxedwine | v86、CheerpX、JSLinux（旧版 Windows） |
| Linux x86/x64 | WSL2、VirtualBox | 原生 | UTM、Lima、VirtualBox | iSH、UTM SE | — | v86、JSLinux、CheerpX |
| Linux arm64 | WSL2 | 原生 | UTM、Lima、VirtualBox | UTM SE | — | — |
| macOS 应用 | — | Darling | 原生；Intel 应用经 Rosetta 2 | — | — | — |
| iOS 应用（早期） | touchHLE | touchHLE | touchHLE | 原生（仅旧应用） | touchHLE | — |
| Android 应用 | — | Waydroid、redroid | UTM（Android 虚拟机） | — | 原生 | — |
| DOS、Windows 3.x–9x | DOSBox-X | DOSBox-X | DOSBox-X | — | — | v86、JSLinux |
| GB / GBC / GBA | mGBA、RetroArch | mGBA、RetroArch | mGBA、RetroArch | RetroArch（mGBA 核心） | mGBA、RetroArch | — |
| Nintendo DS | melonDS、RetroArch | melonDS、RetroArch | melonDS、RetroArch | RetroArch（取决于核心） | melonDS、RetroArch | — |
| Nintendo 64 | RetroArch；Zelda64Recomp | RetroArch；Zelda64Recomp | RetroArch；Zelda64Recomp | RetroArch（取决于核心） | RetroArch | — |
| GameCube / Wii | Dolphin | Dolphin | Dolphin | — | Dolphin | — |
| Wii U | Cemu | Cemu | *Cemu* | — | — | — |
| Switch 1 | Ryujinx、Eden | Ryujinx、Eden | Ryujinx、Eden | *nxnative（少量游戏）* | Eden | — |
| PS1 / PS2 | PCSX2、RetroArch | PCSX2、RetroArch | PCSX2、RetroArch | RetroArch（PS1） | RetroArch（PS1） | — |
| PS3 / PS4 | RPCS3、shadPS4 | RPCS3、shadPS4 | RPCS3、shadPS4 | — | — | — |
| PS5 | *KytyPS5、SharpEmu* | *KytyPS5、SharpEmu* | *KytyPS5、SharpEmu* | — | — | — |
| Xbox / Xbox 360 | xemu；重编译移植 | xemu；重编译移植 | xemu；重编译移植 | — | — | — |
| WebAssembly | Wasmtime | Wasmtime | Wasmtime | — | — | 原生 |

### 主机宿主

这些路线需要自制系统、破解，或两者兼有；平台厂商不支持它们。

| Guest | Switch 1（自制系统） | PS5（已破解） |
| --- | --- | --- |
| GBA / GBC / GB | mGBA、RetroArch | PS5 RetroArch（mGBA 核心） |
| Nintendo DS | RetroArch（取决于核心） | — |
| Nintendo 64 | RetroArch（取决于核心） | — |
| Switch 1 | 原生 | *ProsperoEden* |
| PS1 / PS2 | RetroArch（PS1） | PS5 RetroArch（LRPS2） |
| PSP | RetroArch（取决于核心） | RetroArch（取决于核心） |
| Linux | Switchroot L4T Linux、Lakka | ps5-linux |
| Windows | *通过 Switchroot Linux 使用 Box64 + Wine* | *通过 ps5-linux 使用 Steam/Proton* |
| Android | Switchroot Android | — |

## 兼容层

### Wine

**路线：** Windows x86/x64 与 Windows 3.x/9x 软件 → Linux、macOS 及其他类 Unix 系统 · **状态：** 成熟 · **类型：** API 兼容层

Wine 把 Windows NT 的用户态与内核接口重新实现为 Unix 库，使 Windows PE 二进制无需安装 Windows 即可运行。它是 Proton、CrossOver、Hangover 与 Winlator 的基础。在 Arm 宿主上，Wine 需要搭配 FEX、Box64 或 Rosetta 2，因为 Wine 本身不翻译 x86 指令。反作弊、内核驱动、部分 DRM 与深度集成的 Windows 组件仍是常见失败点。

[官网](https://www.winehq.org/) · [源码](https://github.com/wine-mirror/wine)

### Proton

**路线：** Windows 游戏 → Linux（Steam Deck 与桌面 Steam） · **状态：** 成熟 · **类型：** 带图形翻译的 Wine 发行版

Proton 将 Wine 与 DXVK、VKD3D-Proton、FAudio 及游戏专用补丁打包，并与 Steam Play 集成。它是 Linux 上运行大多数 Windows 游戏的默认路线，但不是通用 Windows 桌面，带内核反作弊或不支持启动器的游戏仍可能失败。Valve 的 README 明确指出，Arm64 Proton 面向 Arm Steam，而不是通过 FEX 运行的 x86 Steam。

[源码](https://github.com/ValveSoftware/Proton)

### CrossOver

**路线：** Windows 应用与游戏 → macOS（Intel 与 Apple silicon）及 Linux · **状态：** 专有 · **类型：** 商业 Wine 发行版

CrossOver 是 CodeWeavers 提供支持的 Wine 版本，带有安装器、完善界面与针对应用的修复。在 Apple silicon 上，它通过 Rosetta 2 运行 x86 Windows 二进制，并可使用 Apple 的 D3DMetal 或基于 Vulkan 的图形路径。它是在 Mac 上运行许多 Windows 生产力应用最省心的方式，但属于付费软件，也无法解决反作弊或驱动级依赖。

[官网](https://www.codeweavers.com/crossover)

### Boxedwine

**路线：** 16/32 位 Windows 应用 → Windows、macOS、Linux 与 Web · **状态：** 活跃 · **类型：** CPU 模拟加 Wine

Boxedwine 模拟 32 位 Linux 内核与 CPU，再在其中运行 32 位 Wine。它通过宿主库支持 OpenGL、Direct3D 与 Vulkan，并提供 Emscripten/Web 构建。它适合在现代 64 位系统上运行旧的 32 位 Windows 软件，但仅支持 32 位，且带有模拟开销；Web 构建没有 JIT，速度较慢。

[源码](https://github.com/danoon2/Boxedwine)

### Winlator

**路线：** Windows x86/x64 应用 → Arm Android · **状态：** 活跃 · **类型：** 打包 Wine 与 Box86/Box64 的 Android 应用

Winlator 将 Wine、Box86/Box64 与图形驱动打包成 Android 应用，用于运行 Windows 游戏与工具。它在 Snapdragon/Adreno 设备上效果最好，更适合较旧或较简单的游戏。需要预期散热限制、驱动相关错误、不支持内核反作弊，以及并不轻松的配置过程。

[源码](https://github.com/brunodev85/winlator)

### Hangover

**路线：** Windows x86/x64 应用 → Arm64 Linux · **状态：** 活跃 · **类型：** 使用 ARM64EC 与 FEX 的 Wine WoW64

Hangover 使用 ARM64EC 与模拟器 DLL，在 Arm64 Wine 上运行 x86 与 x86-64 Windows 应用。只有应用代码被模拟；Windows 与 Unix 调用在模拟器之外原生执行。它也能运行原生 Arm64 Windows 二进制。仅支持 Arm64 Linux，DXVK 需要 Vulkan 1.3 驱动，且项目明确处于实验阶段。

[源码](https://github.com/AndreRH/hangover)

### Darling

**路线：** macOS 应用 → Linux · **状态：** 实验性 · **类型：** Darwin/macOS 兼容层

Darling 在 Linux 之上实现 macOS 的 Mach IPC、POSIX 与 Darwin 系统调用接口，并重新实现 Apple 框架。它使用类似 Wine prefix 的 DPREFIX。命令行工具是目前最可用的目标；GUI、AppKit 与 Metal-on-Vulkan 后端仍在开发中。它尚不支持 `.mpkg` 安装包，且 prefix 需要 overlayfs。

[源码](https://github.com/darlinghq/darling)

### touchHLE

**路线：** 早期 iOS 应用（iPhone OS 2.x–4.0） → Windows、macOS、Android 与 Linux 构建 · **状态：** 活跃 · **类型：** 高级模拟器

touchHLE 不模拟 iPhone 硬件。它取代 iOS 的位置，自行实现 Foundation、UIKit、OpenGL ES 与 OpenAL，由 Dynarmic 执行 32 位应用代码。它面向早期游戏而非现代应用。兼容性由社区数据库跟踪，但该时期的大多数应用仍无法运行，64 位 iOS 明确不在范围内。

[源码](https://github.com/touchHLE/touchHLE) · [兼容性数据库](https://appdb.touchhle.org/)

### WSL

**路线：** Linux x86/x64 与 arm64 → Windows 10/11 · **状态：** 成熟 · **类型：** 带系统集成的轻量虚拟机

Windows Subsystem for Linux 在轻量虚拟机中运行真实 Linux 内核，并与 Windows 集成文件系统、网络与进程启动。WSLg 增加了 GUI 应用支持。它是在 Windows 上运行 Linux 工具链的默认方式，但不是完整桌面替代品，也不能让任意 Linux 内核模块或硬件直通工作。

[源码](https://github.com/microsoft/WSL) · [文档](https://learn.microsoft.com/windows/wsl/)

### Waydroid

**路线：** Android 应用 → Linux（Arm 与 x86） · **状态：** 活跃 · **类型：** 基于容器的 Android 运行时

Waydroid 使用命名空间在 Linux 容器中启动完整 Android 系统镜像，共享宿主内核。它是 Linux 手机与桌面运行 Android 应用的常见路线。它需要支持 binder 的兼容内核，不附带 Google Play 服务；在 x86 上运行 Arm 应用通常需要 libhoudini 或 libndk 之类的原生桥接。

[源码](https://github.com/waydroid/waydroid)

### libhybris

**路线：** Android 硬件库 → 基于 glibc 的 Linux 发行版 · **状态：** 活跃 · **类型：** 底层兼容胶水

libhybris 让 glibc 系统加载并使用基于 bionic 的 Android 硬件适配层与驱动。它支撑了 Android 派生内核上的 Sailfish OS、Halium 与 Ubuntu Touch。它属于基础设施而非面向用户的模拟器：与发行版高度相关、层次很低，只在宿主内核与硬件匹配时有用。

[源码](https://github.com/libhybris/libhybris)

### redroid

**路线：** Android → 使用 Docker、Podman 或 Kubernetes 的 Linux 宿主 · **状态：** 活跃 · **类型：** 多架构 Android 容器

redroid 在 arm64 与 amd64 Linux 宿主上以容器方式运行 Android，支持 GPU 与可选的 x86 上 Arm 应用原生桥接。它面向云 Android 与远程设备工作负载。它不是桌面 Android 模拟器，不捆绑 Google Mobile Services，宿主内核必须提供所需的 Android 特性。

[源码](https://github.com/remote-android/redroid-doc)

## 二进制翻译与用户态模拟

### FEX

**路线：** x86 与 x86-64 Linux 二进制 → Arm64 Linux · **状态：** 成熟 · **类型：** 带 JIT 的用户态模拟器

FEX 在 Arm64 Linux 上运行 32 位与 64 位 x86 Linux 程序，常与 Wine 或 Proton 搭配运行 Windows 游戏。它把图形等 API 调用转发给宿主库以减少开销。它需要 x86-64 根文件系统，以及 ARMv8.0-A 或更新、带 FEAT_FP 与 FEAT_CRC32 的硬件。性能与兼容性高度依赖应用本身，以及其依赖库被 thunk 的程度。

[源码](https://github.com/FEX-Emu/FEX)

### Box64

**路线：** x86-64 Linux 二进制 → Arm64、RISC-V 64 与 LoongArch Linux · **状态：** 成熟 · **类型：** 带动态重编译器的用户态模拟器

Box64 在非 x86 的 64 位小端宿主上翻译 x86-64 Linux 程序，并包装 libc、SDL、OpenGL、Vulkan 等原生库。其 DynaRec 通常比纯解释快 5–10 倍。它支持 Wine64 与 Proton；32 位 Windows 组件需要 Box86 或 Box32。在低端 Arm 或 RISC-V 板卡上，Unity 游戏与需要 OpenGL 3+ 的作品仍可能很困难。

[源码](https://github.com/ptitSeb/box64) · [兼容性列表](https://box86.org/app/)

### Box86

**路线：** x86 Linux 二进制 → 32 位 Arm Linux（以及带 32 位用户态的 64 位宿主） · **状态：** 活跃 · **类型：** 带动态重编译器的用户态模拟器

Box86 是 Box64 的 32 位姊妹项目。它需要 32 位子系统与工具链，因此常用于多架构 Arm 系统，并与 Box64 搭配处理 32 位 Windows 组件。它可以包装原生库来提速。在 Switchroot Linux 上，由于缺少 Armhf NVIDIA GPU 驱动，带加速的 32 位游戏很难运行。

[源码](https://github.com/ptitSeb/box86)

### Rosetta 2

**路线：** x86-64 macOS 应用 → Apple silicon macOS · **状态：** 成熟，已宣布终止时间表 · **类型：** Apple 二进制翻译器

Rosetta 2 通过提前翻译与 JIT 路径，在 Apple silicon 上运行 x86-64 macOS 应用。它不翻译内核扩展，也不翻译虚拟化 x86-64 平台的虚拟机应用，且不支持 AVX-512。Apple 现在表示完整 Rosetta 支持将在 macOS 27 结束；从 macOS 28 起只保留面向部分年久失修游戏的子集。请围绕这一期限做规划。

[文档](https://developer.apple.com/documentation/apple-silicon/about-the-rosetta-translation-environment) · [2026 支持通知](https://developer.apple.com/news/?id=w5ngl9k2) · [支持文章](https://support.apple.com/en-us/102527)

### Windows on Arm x86/x64 模拟

**路线：** x86 与 x64 Windows 应用 → Arm 上的 Windows 11 · **状态：** 成熟 · **类型：** 操作系统内置模拟

Windows 11 on Arm 透明地模拟 x86 与 x64 用户态应用；近期版本中改进后的模拟器通常被称为 Prism。它无需额外组件，是 Snapdragon 等 Arm PC 上运行旧 Windows 应用的默认路线。内核驱动、反作弊、部分指令集扩展与性能敏感负载仍然容易出问题。

[文档](https://learn.microsoft.com/windows/arm/apps-on-arm-x86-emulation)

### blink

**路线：** x86-64 Linux 二进制 → Linux、macOS 及其他 POSIX 宿主 · **状态：** 活跃 · **类型：** 极简用户态模拟器

blink 是一个体积很小、带 JIT 与解释器的 x86-64-linux 用户态模拟器，设计为 `qemu-x86_64` 的更简单替代品。它可以运行 Linux 二进制，包括完整 chroot，并支持调试与跟踪。它实现的是 Linux ABI 而非完整系统硬件，部分 Linux API（例如 System V 消息队列）不受支持。

[源码](https://github.com/jart/blink)

### iSH

**路线：** x86 Linux 用户空间 → iOS 与 iPadOS · **状态：** 活跃 · **类型：** 带系统调用翻译的用户态 x86 模拟

iSH 通过模拟 x86 并翻译 Linux 系统调用，在 iOS 上运行 Alpine Linux shell。它的解释器使用 threaded code 而非 JIT，因此可以上架 App Store。它适合 shell 工具与小型 Linux 实用程序，但仅支持 32 位 x86，无法使用 GPU 加速，重负载下很慢。

[源码](https://github.com/ish-app/ish) · [App Store](https://apps.apple.com/app/ish-shell/id1436902243)

## 图形 API 翻译

### DXVK

**路线：** Direct3D 8/9/10/11 → Vulkan · **状态：** 成熟 · **类型：** 图形 API 翻译层

DXVK 将 Direct3D 8、9、10、11 翻译为 Vulkan，是 Proton 的核心组件。它支持状态缓存、图形管线库与 HUD，也能在 Windows 上使用。它需要较新的 Vulkan 驱动；Direct3D 12 需要 VKD3D-Proton。驱动错误与反作弊是常见阻碍。

[源码](https://github.com/doitsujin/dxvk) · [驱动支持 Wiki](https://github.com/doitsujin/dxvk/wiki/Driver-support)

### VKD3D-Proton

**路线：** Direct3D 12 → Vulkan · **状态：** 成熟 · **类型：** 图形 API 翻译层

VKD3D-Proton 在 Vulkan 上实现完整 Direct3D 12 API，是 Proton 的 D3D12 后端。它积极使用现代 Vulkan 特性来提升性能与游戏兼容性。它需要 Vulkan 1.3 与较强的 descriptor indexing 支持，并不是独立 vkd3d API 的直接替代品。

[源码](https://github.com/HansKristian-Work/vkd3d-proton)

### MoltenVK

**路线：** Vulkan → macOS、iOS、tvOS 与 visionOS 上的 Metal · **状态：** 遗留/已弃用 · **类型：** Vulkan 可移植性实现

MoltenVK 在 Metal 上实现 Vulkan 1.4，并将 SPIR-V 着色器转换为 Metal Shading Language。它仍被模拟器与跨平台引擎广泛使用。Khronos 已将其弃用，仓库转为社区尽力维护；它仍是 Intel Mac 与 iOS 的推荐选项，但在 Apple silicon Mac 上，KosmicKrisp 现在是首选。

[源码](https://github.com/KhronosGroup/MoltenVK)

### KosmicKrisp

**路线：** Vulkan → Apple silicon macOS 26+ 上的 Metal 4 · **状态：** 活跃 · **类型：** Mesa Vulkan 驱动

KosmicKrisp 是 LunarG 基于 Mesa 的 Vulkan 驱动，目标后端是 Apple silicon 上的 Metal。它符合 Vulkan 1.4，随 LunarG Vulkan SDK 提供，是现代 Apple silicon 上运行 DXVK 与 VKD3D-Proton 的推荐 Vulkan 路径。它不支持 Intel Mac，需要 macOS 26+ 与 Metal 4，其 iOS 构建尚未在 Mesa 树中启用。

[文档](https://docs.mesa3d.org/drivers/kosmickrisp.html) · [Vulkan SDK](https://vulkan.lunarg.com/)

### ANGLE

**路线：** OpenGL ES 2.0/3.0/3.1 → Vulkan、Direct3D 9/11、桌面 OpenGL、OpenGL ES、Metal 或 WebGPU · **状态：** 成熟 · **类型：** 图形 API 翻译层

ANGLE 将 OpenGL ES 调用翻译为宿主图形 API。它是 Windows 上 Chrome 与 Firefox 的默认 WebGL 后端，也广泛用于浏览器、模拟器与应用。它实现的是 OpenGL ES，而不是完整桌面 OpenGL API，不同后端的特性支持也不同。

[源码](https://github.com/google/angle)

### Zink

**路线：** OpenGL → Vulkan · **状态：** 活跃 · **类型：** Mesa Gallium 驱动

Zink 是 Mesa 的 Gallium 驱动，发出 Vulkan 调用而不是直接面向 GPU。它在只暴露 Vulkan 的设备上提供桌面 OpenGL，也可通过 Vulkan-on-Metal 驱动用于 Apple silicon。可达到的 OpenGL 版本取决于底层 Vulkan 驱动的特性，性能通常低于原生 OpenGL 驱动。

[文档](https://docs.mesa3d.org/drivers/zink.html)

### Game Porting Toolkit

**路线：** Windows 游戏 → Apple silicon macOS（评估）；Windows 游戏代码 → 原生 Mac 移植 · **状态：** 专有 · **类型：** Apple 移植工具包

Apple 的 Game Porting Toolkit 组合了 D3DMetal（Direct3D 9/10/11/12 到 Metal）、Metal 着色器转换器、性能分析工具，以及用于运行 Windows 游戏二进制的 Wine 评估环境。它的用途是在制作原生移植前评估性能与验证着色器，而不是作为可发行的 Windows 兼容层。不同作品结果差异很大，反作弊与启动器仍是障碍。

[文档](https://developer.apple.com/games/game-porting-toolkit/)

## 虚拟机与全系统模拟

### QEMU

**路线：** 任意受支持的 Guest 架构 → 多种宿主系统；全系统与 Linux 用户态 · **状态：** 成熟 · **类型：** 模拟器与虚拟机监视器

QEMU 可以模拟完整机器，也可以为另一种架构运行 Linux 用户态二进制。配合 KVM、HVF 或其他 hypervisor 时，同架构 Guest 可接近原生速度；没有加速时使用 TCG 动态翻译。全系统性能与设备模拟差异很大，用户态 QEMU 只覆盖 Linux ABI。

[官网](https://www.qemu.org/) · [源码](https://github.com/qemu/qemu)

### UTM

**路线：** Windows、Linux 及其他操作系统 → macOS、iOS、iPadOS 与 visionOS · **状态：** 活跃 · **类型：** QEMU 前端与虚拟机宿主

UTM 以原生应用封装 QEMU 与 Apple Virtualization 框架。它支持 30 多种处理器类型，跨架构 Guest 使用 QEMU TCG，并可在 Apple silicon 上通过 Virtualization.framework 启动 macOS Guest。在 iOS 上，JIT 需要越狱或特定绕过方法；UTM SE 使用解释器，速度较慢，但可在未修改设备上侧载。

[源码](https://github.com/utmapp/UTM) · [官网](https://getutm.app/)

### VirtualBox

**路线：** x86/x64 Guest → Windows、Linux 与 Intel macOS；Arm Guest → Apple silicon macOS 与 Windows on Arm（7.2+） · **状态：** 成熟 · **类型：** Hypervisor

VirtualBox 是通用桌面 hypervisor。7.2 版本新增 Apple silicon macOS 与 Windows on Arm 作为宿主，并支持 Windows 11 Arm Guest。它并不官方支持在 Arm 宿主上模拟 x86 Guest；那种场景应使用 UTM 或 QEMU。图形加速与 Guest Additions 因宿主和 Guest 组合而异。

[官网](https://www.virtualbox.org/) · [7.2 公告](https://blogs.oracle.com/virtualization/oracle-virtualbox-72)

### Lima

**路线：** Linux 虚拟机 → macOS、Linux 与 NetBSD 宿主 · **状态：** 活跃 · **类型：** Linux 虚拟机管理器

Lima 启动带自动文件共享与端口转发的 Linux 虚拟机，理念类似 macOS 上的 WSL2。它支持 containerd、Docker、Podman 与 Kubernetes，底层使用 QEMU 或 Apple Virtualization 框架。它只支持 Linux Guest，且以命令行为主，因此不是运行 Windows 或 macOS Guest 的路线。

[源码](https://github.com/lima-vm/lima) · [官网](https://lima-vm.io/)

## 浏览器与 WebAssembly

### v86

**路线：** x86 PC 操作系统 → Web 浏览器 · **状态：** 活跃 · **类型：** x86 到 WebAssembly 的 JIT

v86 在浏览器中模拟兼容 x86 的 CPU 与 PC 硬件。它可以启动 32 位 Linux 发行版、DOS、Windows 1.01 到 2000、Android-x86 及其他 x86 系统。它是完整的 PC 模拟器，而不是 Linux ABI 层。它不支持 64 位内核，性能一般，也没有 GPU 直通。

[源码](https://github.com/copy/v86) · [演示](https://copy.sh/v86/)

### CheerpX

**路线：** 32 位 x86 Linux 二进制与发行版 → Web 浏览器 · **状态：** 专有 · **类型：** WebAssembly 中的 x86 虚拟化

CheerpX 在 WebAssembly 沙箱中运行未修改的 x86 Linux 二进制，实现 Linux 系统调用，并可启动完整发行版。它面向浏览器中安全执行不可信代码。它只支持 32 位 x86，不提供 3D 加速，商业使用需要许可证。

[源码](https://github.com/leaningtech/cheerpx-meta) · [官网](https://leaningtech.com/cheerpx/)

### JSLinux

**路线：** x86/x86-64 与 RISC-V Linux、Windows 2000、FreeDOS → Web 浏览器 · **状态：** 活跃 · **类型：** 浏览器 PC 模拟器

JSLinux 是 Fabrice Bellard 的浏览器内模拟系统合集。它包括支持 AVX-512 与 APX 的 x86_64 Alpine Linux、带图形环境的 32 位 Alpine、Windows 2000、FreeDOS 与 RISC-V Linux 镜像。它是技术演示与轻量沙箱，不是通用桌面替代品。

[官网](https://bellard.org/jslinux/)

### Wasmtime

**路线：** WebAssembly 模块 → Windows、Linux 与 macOS；可嵌入应用 · **状态：** 成熟 · **类型：** WebAssembly 运行时

Wasmtime 是 Bytecode Alliance 提供的高速、安全、符合标准的 WebAssembly 运行时。它支持 WASI，可在浏览器之外运行 Wasm 工作负载，或嵌入宿主应用。它运行的是 WebAssembly，而不是直接运行 x86 或操作系统二进制；你需要一个编译到 Wasm 的 Guest，或像 v86 这样编译到 Wasm 的运行时。

[源码](https://github.com/bytecodealliance/wasmtime) · [官网](https://wasmtime.dev/)

## 游戏主机与电脑模拟器

### Dolphin

**路线：** GameCube 与 Wii → Windows、Linux、macOS 与较新 Android · **状态：** 成熟 · **类型：** 主机模拟器

Dolphin 模拟 GameCube 与 Wii，带有 x86-64 与 Arm64 JIT 重编译器，支持 Direct3D 11/12、OpenGL、Vulkan 与 Metal 后端。它是最成熟的主机模拟器之一，并提供更高分辨率、宽屏等增强。Android 性能高度依赖 GPU 驱动；没有官方 iOS 构建。

[源码](https://github.com/dolphin-emu/dolphin) · [官网](https://dolphin-emu.org/)

### PCSX2

**路线：** PlayStation 2 → Windows、Linux 与 macOS · **状态：** 成熟 · **类型：** 主机模拟器

PCSX2 已有 20 多年开发历史，能以全速运行大多数 PS2 游戏，兼容性数据库测试了 2,500 多款作品。它需要 PS2 BIOS 转储，并提供 Vulkan、Direct3D 12 与 OpenGL 后端。通常需要逐游戏设置；安装前请查看当前 macOS 要求。

[源码](https://github.com/PCSX2/pcsx2) · [官网](https://pcsx2.net/) · [兼容性](https://pcsx2.net/compat/)

### RPCS3

**路线：** PlayStation 3 → Windows、Linux、macOS 与 FreeBSD · **状态：** 成熟 · **类型：** 主机模拟器与调试器

RPCS3 是领先的开源 PS3 模拟器，带有 PPU 与 SPU 重编译器以及 Vulkan 渲染器。它对 CPU 要求较高，兼容性因作品而异，但兼容性列表非常庞大。它需要 PS3 固件与合法获取的游戏转储。

[源码](https://github.com/RPCS3/rpcs3) · [官网](https://rpcs3.net/)

### shadPS4

**路线：** PlayStation 4 → Windows、Linux 与 Apple silicon macOS · **状态：** 活跃 · **类型：** 主机模拟器

shadPS4 是一个用 C++ 编写的早期 PS4 模拟器。它执行 PS4 的 x86-64 代码，并使用 Vulkan 渲染，配有社区兼容性列表。许多游戏仍无法启动或正确渲染。macOS 构建需要 macOS 26.0 或更高，且不支持 Intel Mac。

[源码](https://github.com/shadps4-emu/shadPS4) · [兼容性](https://github.com/shadps4-compatibility/shadps4-game-compatibility)

### KytyPS5

**路线：** PlayStation 5 → Windows 与 Linux；macOS 通过 Rosetta 2 与 MoltenVK 实验性支持 · **状态：** 实验性 · **类型：** PS5 翻译层

KytyPS5 是免费开源的 PS5 翻译层，源自 Kyty。它执行原生 PS5 代码并以 Vulkan 1.3 为目标，主要平台是 Windows 与 Linux。部分 2D 与 Unreal/Unity 作品可以进入菜单或游戏，但大多数作品运行不佳。macOS 构建为 x86-64，在 Apple silicon 上通过 Rosetta 2 运行。

[源码](https://github.com/KytyPS5/KytyPS5) · [官网](https://kytyps5.github.io/)

### SharpEmu

**路线：** PlayStation 5 → Windows、Linux 与 macOS（x86-64，Apple silicon 上经 Rosetta 2） · **状态：** 实验性 · **类型：** PS5 模拟器

SharpEmu 是一个用 C# 从零编写的实验性 PS5 模拟器。它可以加载 `eboot.bin`、执行原生 CPU 指令并部分处理 GPU 工作，Windows/Linux 使用 Vulkan，macOS 使用 MoltenVK。兼容性有限，项目重点在准确性与基础设施，而不是完成度高的游戏体验。

[源码](https://github.com/sharpemu/sharpemu) · [兼容性](https://sharpemu.app/compatibility/)

### xemu

**路线：** 初代 Xbox → Windows、Linux 与 macOS · **状态：** 活跃 · **类型：** 主机模拟器

xemu 模拟初代 Xbox，包括其 NVIDIA GPU 与 Pentium III CPU。它需要 Xbox BIOS/MCPX 启动 ROM 与游戏转储，支持 Vulkan 与 OpenGL 渲染。它不能运行 Xbox 360 软件；后者应使用基于 XenonRecomp 的移植。

[源码](https://github.com/xemu-project/xemu) · [官网](https://xemu.app/)

### PPSSPP

**路线：** PlayStation Portable → Windows、macOS、Linux、Android 与 iOS · **状态：** 成熟 · **类型：** 主机模拟器

PPSSPP 是高级 PSP 模拟器，不需要 BIOS。它支持 Vulkan、OpenGL 与 Direct3D 11，并有官方 Arm64 Windows 构建。大多数作品兼容性良好，项目维护社区兼容性数据库。iOS 构建受 JIT 与 App Store 规则限制。

[源码](https://github.com/hrydgard/ppsspp) · [官网](https://www.ppsspp.org/) · [兼容性](https://report.ppsspp.org/games)

### melonDS

**路线：** Nintendo DS → Windows、Linux、macOS 与 Android · **状态：** 活跃 · **类型：** 主机模拟器

melonDS 模拟 Nintendo DS 与 DSi，带有 OpenGL 渲染器，支持直接启动游戏或固件启动。固件启动需要从原版 DS 或 DS Lite 转储的 BIOS/固件；DSi 与 3DS 固件转储只适合直接启动。部分 DSi 特性与 Wi-Fi 行为仍不完整。

[源码](https://github.com/melonDS-emu/melonDS) · [官网](https://melonds.kuribo64.net/)

### mGBA

**路线：** Game Boy、Game Boy Color 与 Game Boy Advance → Windows、macOS、Linux、Android、Switch、3DS、Vita、Wii，以及通过 RetroArch 到 PS5 · **状态：** 成熟 · **类型：** 主机模拟器

mGBA 是精确的 GBA 模拟器，也支持 GB 与 GBC 游戏。它提供联机线、实时时钟、太阳能传感器、Game Boy Camera/Printer、e-Reader、Lua 脚本与 GDB 调试。少数 mapper 仅部分支持。它是 RetroArch 推荐的 GBA 核心，也是 GBA 游戏进入 Switch 与 PS5 的方式。

[源码](https://github.com/mgba-emu/mgba) · [官网](https://mgba.io/) · [下载](https://mgba.io/downloads.html)

### DOSBox-X

**路线：** DOS、Windows 3.x/9x/ME 与 NEC PC-98 软件 → Windows、Linux 与 macOS · **状态：** 活跃 · **类型：** PC 模拟器

DOSBox-X 是 DOSBox 的分支，侧重精确模拟与可配置性。它运行 DOS 游戏与应用、Windows 3.x/9x/ME、DOS/V 与 PC-98 软件，并暴露大量硬件与配置选项。它不是通用 Windows 应用运行器，精确模拟也很吃 CPU。

[源码](https://github.com/joncampbell123/dosbox-x) · [官网](https://dosbox-x.com/)

### Cemu

**路线：** Wii U → Windows、Linux 与 macOS（实验性） · **状态：** 活跃 · **类型：** 主机模拟器

Cemu 模拟 Wii U，支持 Vulkan 与 OpenGL 渲染。原生 macOS 构建仍是实验性的，已知在 Apple silicon 上因 MoltenVK 与 Rosetta 存在性能问题。它需要合法转储的游戏文件与密钥，在线功能不是优先事项。

[源码](https://github.com/cemu-project/Cemu) · [官网](https://cemu.info/)

### RetroArch

**路线：** 多种经典主机与电脑 → Windows、Linux、macOS、Android、iOS、Switch、PS5 自制系统等 · **状态：** 成熟 · **类型：** libretro 前端

RetroArch 是 libretro API 的主要前端，拥有 NES、SNES、GB/GBC/GBA、DS、N64、PlayStation、PS2、PSP、街机、DOS 等核心。它提供着色器、联机、倒带与统一的控制器界面。核心质量参差不齐，部分核心需要 BIOS/固件，iOS App Store 版本受 JIT 规则限制。PS5 移植是 alpha 自制项目。

[源码](https://github.com/libretro/RetroArch) · [官网](https://www.retroarch.com/) · [核心](https://docs.libretro.com/)

### Ryujinx

**路线：** Nintendo Switch 1 → Windows、Linux 与 macOS · **状态：** 遗留（社区分支活跃） · **类型：** 主机模拟器

Ryujinx 是以准确性著称的 C# Nintendo Switch 模拟器。原项目于 2024 年 10 月停止；Ryubing 社区分支在 `git.ryujinx.app` 继续开发，支持 Vulkan、OpenGL 与 Metal（通过 MoltenVK），兼容性列表测试了约 4,300 款作品。它需要从自己的主机转储的 Switch 固件与密钥。没有官方 Android 构建。

[官网](https://ryujinx.app/) · [源码（Ryubing 分支）](https://git.ryujinx.app/projects/Ryubing)

### Eden

**路线：** Nintendo Switch 1 → Windows、Linux、macOS 与 Android · **状态：** 活跃 · **类型：** 主机模拟器

Eden 是源自 Yuzu 的社区 Switch 模拟器。在任天堂 DMCA 下架使项目离开 GitHub 后，它在自己的 Forgejo 实例上开发，提供 Vulkan 渲染与 Android 构建。它需要固件与密钥，兼容性因作品而异，没有 iOS 构建。

[官网](https://eden-emu.dev/) · [源码](https://git.eden-emu.dev/eden-emu/eden)

## 游戏引擎重实现

### ScummVM

**路线：** 经典冒险游戏与受支持的引擎 → Windows、macOS、Linux、Android、iOS 与主机 · **状态：** 成熟 · **类型：** 游戏引擎重实现

ScummVM 重实现了数百款冒险游戏的引擎，包括 SCUMM、Sierra 及其他经典引擎，并运行原始游戏数据。它不是模拟器，也不包含游戏。兼容性按作品跟踪，只有受支持的游戏才能运行。

[源码](https://github.com/scummvm/scummvm) · [官网](https://www.scummvm.org/) · [兼容性](https://www.scummvm.org/compatibility/)

### OpenMW

**路线：** The Elder Scrolls III: Morrowind → Windows、macOS 与 Linux · **状态：** 成熟 · **类型：** 游戏引擎重实现

OpenMW 是 Morrowind 引擎的开源重实现。它加载原版游戏数据，增加改进的图形与界面，并支持 Mod，但由于 OpenMW 的脚本编译器比原版更严格，Mod 兼容性并不完美。它需要合法的 Morrowind 安装，且不支持 32 位平台。

[源码](https://github.com/OpenMW/openmw) · [官网](https://openmw.org/)

## 静态重编译与原生移植

### N64Recomp

**路线：** Nintendo 64 二进制 → C 代码 → 任意平台的原生可执行文件 · **状态：** 活跃 · **类型：** 静态重编译器

N64Recomp 将 N64 机器码翻译为可编译到现代平台的 C 代码。它支持可重定位 overlay 与 RSP 微码，是 Zelda 64: Recompiled 的基础。它是用于制作移植的工具，而不是通用模拟器：每款游戏都需要元数据、分析，通常还需要运行时工作。

[源码](https://github.com/N64Recomp/N64Recomp)

### Zelda64Recomp

**路线：** The Legend of Zelda: Majora's Mask（以及开发中的 Ocarina of Time） → Windows、Linux 与 macOS · **状态：** 活跃 · **类型：** 静态重编译移植

Zelda 64: Recompiled 使用 N64Recomp 将 Majora's Mask 转为原生移植，通过 RT64 渲染器带来高帧率、宽屏支持与 Mod。它需要北美版 ROM，以及支持 Direct3D 12、Vulkan 1.2 或 Metal Argument Buffers Tier 2 的 GPU。Ocarina of Time 支持仍在开发中。

[源码](https://github.com/Zelda64Recomp/Zelda64Recomp) · [官网](https://zelda64.dev/)

### XenonRecomp

**路线：** Xbox 360 可执行文件 → C++ → 原生可执行文件 · **状态：** 活跃 · **类型：** 静态重编译器

XenonRecomp 将 Xbox 360 PowerPC 可执行文件转换为可编译到现代平台的 C++。许多 VMX 指令使用 x86 intrinsic 或 SIMDe 实现，setjmp/longjmp 被重定向到原生实现。MMIO 与异常尚未完整支持，因此每款游戏都需要额外的逆向工作。

[源码](https://github.com/hedge-dev/XenonRecomp)

### UnleashedRecomp

**路线：** Sonic Unleashed（Xbox 360） → Windows 与 Linux · **状态：** 活跃 · **类型：** 静态重编译移植

Unleashed Recompiled 是 Xbox 360 版 Sonic Unleashed 的非官方 PC 移植，使用 XenonRecomp 与 XenosRecomp 构建。它增加了高帧率、超宽屏、性能改进与 Mod。它需要美版或欧版游戏文件；日版不受支持，且 CPU 必须支持 AVX。

[源码](https://github.com/hedge-dev/UnleashedRecomp)

## 主机作为宿主

### nxnative

**路线：** Nintendo Switch 1 游戏 → iPhone 与 iPad（原生执行，不是模拟） · **状态：** 实验性 · **类型：** 原生移植工具链

nxnative 重写 Switch NSO 二进制、替代 Horizon OS，并将 NVN 图形翻译为 Metal，从 NSP 文件夹与 `prod.keys` 生成可侧载的 IPA。它需要 iOS/iPadOS 26 或更高，以及 A17 Pro 或 M1 及更新设备，因为游戏使用 BC 压缩纹理。仅支持少量作品，且不包含任何游戏数据。

[源码](https://github.com/seedds/nxnative)

### ProsperoEden

**路线：** Nintendo Switch 1 游戏 → 已破解 PlayStation 5 · **状态：** 实验性 · **类型：** Eden 模拟器的 PS5 移植

ProsperoEden 是 Eden 的非官方 PS5 移植，以原生 PS5 自制应用形式打包。它使用运行在 PS5 Mesa/RADV 移植上的 Vulkan 渲染器，需要破解以及自己的固件、密钥与游戏文件。早期报告显示较轻的作品约 40 FPS，而较重的作品很吃力。它是 alpha 软件，不能使用 PSN。

[源码](https://github.com/Swordpdf/ProsperoEden)

### PS5 RetroArch

**路线：** GBA、DS、PS1、PS2、PSP 及其他经典系统 → 已破解 PlayStation 5 · **状态：** 实验性 · **类型：** 原生 RetroArch 自制应用

PS5 RetroArch 是面向已破解 PS5 的原生 RetroArch 自制应用，带原生 Vulkan 渲染、十五个发布核心与本地 WebUI。核心列表包括用于 Game Boy/GBC/GBA 的 mGBA 与用于 PlayStation 2 的 LRPS2。它是 alpha 版本，需要破解及自己的内容/BIOS/固件，不能运行 PS5 游戏。

[源码](https://github.com/mihawk-99/PS5_RetroArch) · [镜像](https://github.com/SocialMDev/ps5_retroarch)

### ps5-linux

**路线：** Linux → 固件 3.00–13.60 的 PlayStation 5 Phat、Slim 与 Pro · **状态：** 实验性 · **类型：** Hypervisor 漏洞与 Linux 引导器

ps5-linux 利用已修补的 hypervisor 漏洞，在 PS5 上启动自定义 Linux 镜像，把主机变成拥有八核十六线程并可访问 GPU 的 Linux PC。它支持 USB 与 M.2 安装，可运行 Steam 游戏与模拟器。它需要特定固件的已破解主机，不能降级，并会牺牲 PSN 访问。项目负责人在 2026 年离开了该社区，因此长期支持存在不确定性。

[源码](https://github.com/ps5-linux/ps5-linux-loader)

### Lakka

**路线：** RetroArch/Linux → Nintendo Switch 1（Switchroot L4T Lakka beta）、PC 与单板计算机 · **状态：** 活跃 · **类型：** 基于 RetroArch 的 Linux 发行版

Lakka 是围绕 RetroArch 构建的轻量 LibreELEC 发行版。在 Switch 1 上，它通过 Switchroot 与 hekate 安装在未打补丁的主机上，是把主机变成 RetroArch 宿主的方式之一。Switchroot 还提供 Android、L4T Ubuntu 与 L4T Fedora。这些路线属于社区项目：需要未打补丁的主机，可能不稳定，且不受任天堂支持。

[官网](https://www.lakka.tv/) · [Switch 指南](https://www.lakka.tv/doc/Nintendo-Switch/) · [Switchroot FAQ](https://wiki.switchroot.org/wiki/faq)

## 路线配方

### 在 Apple silicon 上运行 Windows 游戏

1. 如系统提示，安装 Rosetta 2。
2. 需要最省心的受支持路径时使用 **CrossOver**；需要测试时使用 **Game Porting Toolkit** 评估环境。
3. 让 CrossOver/GPTK 选择 D3DMetal，或在 **KosmicKrisp**（Apple silicon，macOS 26+）或 **MoltenVK**（Intel Mac 与较旧环境）之上，将 Wine 与 **DXVK**/**VKD3D-Proton** 搭配。
4. 预期失败点是反作弊、启动器与媒体编解码器。这不是通用的 Windows 兼容层。

### 在 Arm Linux 上运行 Windows 应用

- 对 x86 与 x86-64 Windows 二进制，使用 **FEX** 搭配 Wine/Proton。
- 需要 Wine WoW64 加 ARM64EC 与 FEX 模拟器 DLL 时，使用 **Hangover**。
- 对 x86-64 二进制使用 **Box64** + Wine64，对 32 位组件使用 **Box86**/Box32。
- 保持现实预期：Vulkan 1.3 驱动、被 thunk 的库与逐应用修复，比单纯 CPU 速度更重要。

### 在 Android 上运行 Windows 应用

- 安装 **Winlator**，让它创建带 Wine 与 Box86/Box64 的容器。
- 优先选择较旧或较简单的游戏；Snapdragon/Adreno 设备的驱动支持最好。
- 没有应对内核反作弊的通用方案，热降频也很常见。

### 在 Windows 上运行 Linux

- 使用 **WSL2** 运行命令行工具、开发环境与通过 WSLg 的 GUI 应用。
- 需要完整桌面 Linux 虚拟机时，使用 **VirtualBox** 或其他 hypervisor。
- WSL2 不是完整桌面替代；GPU 计算与嵌套虚拟化取决于 Windows 版本与硬件。

### 在 Mac 上运行 Linux

- 对 arm64 Linux 使用 **UTM** 或 **Lima**。在 Apple silicon 上，arm64 Guest 可通过 Apple Virtualization 框架接近原生运行。
- 对 x86 Linux Guest，使用 **UTM** 的 QEMU TCG；**VirtualBox** 仅适用于受支持的 Arm Guest。
- 不需要完整虚拟机、只想运行 x86 Linux 二进制时，可在 iOS/iPadOS 上试用 **iSH**（32 位用户态），或在 macOS 上使用 **QEMU** 用户态模式。

### 在 Linux 上运行 Android

- 桌面 Android 体验使用 **Waydroid**。
- 服务器或 CI 上的容器化 Android 实例使用 **redroid**。
- 两者都需要兼容内核，默认不包含 Google Play 服务。

### Switch 1 游戏

- Windows、Linux 与 macOS：**Ryujinx**（Ryubing 分支）或 **Eden**。
- Android：**Eden**（或在有 Android 构建时使用 Ryujinx）。
- iPhone/iPad：**nxnative**，仅支持少量作品，采用原生执行而非模拟。
- 已破解 PS5：**ProsperoEden**。
- 你需要自己的主机密钥、固件与游戏转储。这些项目不提供它们。

### 把 Switch 1 或 PS5 变成宿主

- **Switch 1：** 在未打补丁的主机上安装 Switchroot（Android、L4T Ubuntu/Fedora 或 L4T Lakka）。使用 **Lakka** 或 **RetroArch** 模拟，Box64 + Wine 只适合轻量 Windows 软件。
- **PS5：** 在受支持的固件上启动 **ps5-linux**，然后运行 Steam/Proton 与模拟器；或者留在 PS5 系统内，使用 **PS5 RetroArch** 与 **ProsperoEden** 自制应用。
- 两条路线都需要破解或自制系统，可能破坏在线访问，且不受厂商认可。

### GBA 到处运行

- 桌面、Android 与大多数掌机：直接使用 **mGBA**，或使用带 mGBA 核心的 **RetroArch**。
- iPhone/iPad：使用带 mGBA 核心的 **RetroArch**。
- Switch 1：使用 **mGBA** 或 **RetroArch** 自制应用。
- PS5：使用带 mGBA 核心的 **PS5 RetroArch**。
- 浏览器：使用 WebAssembly GBA 模拟器；本列表暂未收录。

### Xbox 与 Xbox 360

- 初代 Xbox：**xemu**，需要 BIOS/MCPX 启动 ROM 与游戏转储。
- Xbox 360：本列表中没有通用模拟器。**XenonRecomp** 与 **UnleashedRecomp** 展示的是针对特定作品的静态重编译路线。

### 旧 iOS 游戏

- 对 iPhone OS 2.x–4.0 的 32 位应用使用 **touchHLE**。
- 先查兼容性数据库；大多数应用无法运行。
- 现代 64 位 iOS 应用在非 Apple 宿主上没有实用路线。

## 法律与实践说明

- **不包含任何内容。** 本列表不含 ROM、ISO、BIOS、主机固件、解密密钥或游戏数据。在本地法律允许的范围内，你必须自行转储并使用自己拥有的内容。
- **模拟器不等于盗版。** 许多模拟器是合法的，但分发或下载受版权保护的游戏、固件与密钥并不合法。转储与格式转换的合法性因司法辖区而异。
- **破解有代价。** 它可能使保修失效、封锁在线服务，并危及账号。PS5 Linux 与自制路线可能无法使用 PSN，固件更新也可能移除漏洞。
- **反作弊与 DRM 不在范围内。** 本列表不涉及绕过内核反作弊、DRM 或账号限制。
- **性能取决于负载。** 能运行 2D 独立游戏的路线，可能在带反作弊的 3A 作品上失败；更快的 CPU 也无法弥补缺失的 GPU 特性。
- **上游项目会变化。** 状态标签反映 2026-10-24 核验的一手资料。MoltenVK 已弃用，Apple silicon 上转向 KosmicKrisp；Apple 计划在 macOS 27 之后结束完整 Rosetta 支持；Switch 模拟器社区已转向社区分支与 Forgejo。如果路线已变化，请提交 Issue。


## 贡献

欢迎贡献。提交 Pull Request 前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

简要要求：

- 每个 Pull Request 只处理一个项目，附带一手来源与具体的 Guest → Host 路线。
- 将条目加入正确分类；如果是新路线，同时更新矩阵。
- 说明状态与主要限制；不要把实验性项目描述为成熟项目。
- 同时更新 `README.md` 与 `README.zh-CN.md`。
- 不要添加盗版工具、ROM/固件链接或反作弊绕过。
- 运行链接检查，或确认新链接可以访问。

---

以 [MIT 许可证](LICENSE) 授权。项目名称与商标归各自所有者所有。
