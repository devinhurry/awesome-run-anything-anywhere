# Awesome Run Anything Anywhere

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Link check](https://github.com/devinhurry/awesome-run-anything-anywhere/actions/workflows/links.yml/badge.svg)](https://github.com/devinhurry/awesome-run-anything-anywhere/actions/workflows/links.yml)

> A curated map of projects that make software run on hardware or operating systems it was not built for.

**English** · [简体中文](README.zh-CN.md)

This list covers the practical routes between CPU architectures, operating-system families, and the PC/console boundary: API compatibility layers, binary translation, full-system emulation, graphics translation, virtual machines, browser sandboxes, static recompilation, and native ports.

The list is deliberately opinionated. It favours projects that are usable today, records the limits that decide whether a route works, and labels experiments instead of presenting them as finished products. This first edition covers **60 projects**, checked against primary sources on **2026-10-24**.

If you are here for the headline routes — Windows games on Apple silicon, Switch 1 games on PC/Android/PS5, PS5 Linux and PS5 RetroArch, or GBA on everything — start with the [matrix](#guest--host-matrix) and the [route recipes](#route-recipes).

## Contents

- [Scope](#scope)
- [How to read this list](#how-to-read-this-list)
- [Guest → host matrix](#guest--host-matrix)
- [Compatibility layers](#compatibility-layers)
- [Binary translation and userspace emulation](#binary-translation-and-userspace-emulation)
- [Graphics API translation](#graphics-api-translation)
- [Virtual machines and full-system emulation](#virtual-machines-and-full-system-emulation)
- [Browser and WebAssembly](#browser-and-webassembly)
- [Game console and computer emulators](#game-console-and-computer-emulators)
- [Game engine reimplementations](#game-engine-reimplementations)
- [Static recompilation and native ports](#static-recompilation-and-native-ports)
- [Console-as-host routes](#console-as-host-routes)
- [Route recipes](#route-recipes)
- [Legal and practical notes](#legal-and-practical-notes)
- [Contributing](#contributing)

## Scope

**Included**

- Running Windows, Linux, macOS, iOS, Android, DOS, and console software on a different host.
- CPU translation between x86, Arm, RISC-V, PowerPC, and WebAssembly.
- API translation for graphics, audio, input, and operating-system services.
- Virtual machines, emulators, static recompilers, and source-port projects that recreate the original runtime.
- Commercial and closed-source tools when they define the state of the art.
- Experimental and discontinued projects when they are historically important or still uniquely useful.

**Not included**

- Ordinary application containers, remote desktop, and cloud-streaming services.
- Storefronts, launchers, and compatibility-rating databases.
- DRM removal, anti-cheat bypasses, and piracy tools.
- Projects that only redistribute games, firmware, BIOS files, or keys.

## How to read this list

Each entry gives the guest, the host, the mechanism, and the main limitation.

| Label | Meaning |
| --- | --- |
| **Mature** | Widely used and stable for the stated route; compatibility still varies by app. |
| **Active** | Maintained and improving, but compatibility or platform support is uneven. |
| **Experimental** | Early-stage, limited titles or features, and expected breakage. |
| **Legacy** | Discontinued, deprecated, or superseded; still useful for preservation or comparison. |
| **Proprietary** | Closed-source or commercial. Free trials may exist. |

The route is written as **guest → host**. A route can combine tools; for example, *Windows app → Linux on Arm* usually means Wine plus FEX or Box64. The matrix lists the shortest well-known combination, not every possible stack.

## Guest → host matrix

### Desktop, mobile, and browser hosts

A name means a practical route exists. *Italic* means the route is experimental or limited. `—` means no established route in this list.

| Guest | Windows | Linux | macOS | iOS / iPadOS | Android | Browser |
| --- | --- | --- | --- | --- | --- | --- |
| Windows x86/x64 | Native; Prism on Arm | Wine, Proton, CrossOver | CrossOver, Wine + Rosetta, GPTK | — | Winlator, Boxedwine | v86, CheerpX, JSLinux (older Windows) |
| Linux x86/x64 | WSL2, VirtualBox | Native | UTM, Lima, VirtualBox | iSH, UTM SE | — | v86, JSLinux, CheerpX |
| Linux arm64 | WSL2 | Native | UTM, Lima, VirtualBox | UTM SE | — | — |
| macOS apps | — | Darling | Native; Rosetta 2 for Intel apps | — | — | — |
| iOS apps (early) | touchHLE | touchHLE | touchHLE | Native (old apps only) | touchHLE | — |
| Android apps | — | Waydroid, redroid | UTM (Android VM) | — | Native | — |
| DOS, Windows 3.x–9x | DOSBox-X | DOSBox-X | DOSBox-X | — | — | v86, JSLinux |
| GB / GBC / GBA | mGBA, RetroArch | mGBA, RetroArch | mGBA, RetroArch | RetroArch (mGBA core) | mGBA, RetroArch | — |
| Nintendo DS | melonDS, RetroArch | melonDS, RetroArch | melonDS, RetroArch | RetroArch (core-dependent) | melonDS, RetroArch | — |
| Nintendo 64 | RetroArch; Zelda64Recomp | RetroArch; Zelda64Recomp | RetroArch; Zelda64Recomp | RetroArch (core-dependent) | RetroArch | — |
| GameCube / Wii | Dolphin | Dolphin | Dolphin | — | Dolphin | — |
| Wii U | Cemu | Cemu | *Cemu* | — | — | — |
| Switch 1 | Ryujinx, Eden | Ryujinx, Eden | Ryujinx, Eden | *nxnative (few games)* | Eden | — |
| PS1 / PS2 | PCSX2, RetroArch | PCSX2, RetroArch | PCSX2, RetroArch | RetroArch (PS1) | RetroArch (PS1) | — |
| PS3 / PS4 | RPCS3, shadPS4 | RPCS3, shadPS4 | RPCS3, shadPS4 | — | — | — |
| PS5 | *KytyPS5, SharpEmu* | *KytyPS5, SharpEmu* | *KytyPS5, SharpEmu* | — | — | — |
| Xbox / Xbox 360 | xemu; recomp ports | xemu; recomp ports | xemu; recomp ports | — | — | — |
| WebAssembly | Wasmtime | Wasmtime | Wasmtime | — | — | Native |

### Console hosts

These routes require homebrew, a jailbreak, or both. They are not supported by the platform vendors.

| Guest | Switch 1 (homebrew) | PS5 (jailbroken) |
| --- | --- | --- |
| GBA / GBC / GB | mGBA, RetroArch | PS5 RetroArch (mGBA core) |
| Nintendo DS | RetroArch (core-dependent) | — |
| Nintendo 64 | RetroArch (core-dependent) | — |
| Switch 1 | Native | *ProsperoEden* |
| PS1 / PS2 | RetroArch (PS1) | PS5 RetroArch (LRPS2) |
| PSP | RetroArch (core-dependent) | RetroArch (core-dependent) |
| Linux | Switchroot L4T Linux, Lakka | ps5-linux |
| Windows | *Box64 + Wine via Switchroot Linux* | *Steam/Proton via ps5-linux* |
| Android | Switchroot Android | — |

## Compatibility layers

### Wine

**Route:** Windows x86/x64 and Windows 3.x/9x software → Linux, macOS, and other Unix-like systems · **Status:** Mature · **Type:** API compatibility layer

Wine reimplements the Windows NT user and kernel interfaces as Unix libraries, so Windows PE binaries can run without a Windows installation. It is the foundation of Proton, CrossOver, Hangover, and Winlator. On Arm hosts, pair Wine with FEX, Box64, or Rosetta 2; Wine itself does not translate x86 instructions. Anti-cheat, kernel drivers, some DRM, and tightly integrated Windows components remain common failures.

[Website](https://www.winehq.org/) · [Source](https://github.com/wine-mirror/wine)

### Proton

**Route:** Windows games → Linux (Steam Deck and desktop Steam) · **Status:** Mature · **Type:** Wine distribution with graphics translation

Proton bundles Wine with DXVK, VKD3D-Proton, FAudio, and game-specific patches, and integrates with Steam Play. It is the default route for most Windows games on Linux. It is not a general-purpose Windows desktop, and games with kernel anti-cheat or unsupported launchers may still fail. Valve's own README notes that Arm64 Proton builds are intended for Arm Steam, not x86 Steam running through FEX.

[Source](https://github.com/ValveSoftware/Proton)

### CrossOver

**Route:** Windows applications and games → macOS (Intel and Apple silicon) and Linux · **Status:** Proprietary · **Type:** Commercial Wine distribution

CrossOver is CodeWeavers' supported Wine build with installers, a polished UI, and per-application fixes. On Apple silicon it runs x86 Windows binaries through Rosetta 2 and can use Apple's D3DMetal or Vulkan-based graphics paths. It is the least fiddly way to run many Windows productivity apps on a Mac, but it is paid software and does not solve anti-cheat or driver-level dependencies.

[Website](https://www.codeweavers.com/crossover)

### Boxedwine

**Route:** 16/32-bit Windows applications → Windows, macOS, Linux, and the web · **Status:** Active · **Type:** CPU emulation plus Wine

Boxedwine emulates a 32-bit Linux kernel and CPU, then runs a 32-bit Wine build inside that environment. It supports OpenGL, Direct3D, and Vulkan through host libraries, and has an Emscripten/Web build. It is useful for old 32-bit Windows software on modern 64-bit systems, but it is 32-bit only and carries emulation overhead; the web build has no JIT and is slow.

[Source](https://github.com/danoon2/Boxedwine)

### Winlator

**Route:** Windows x86/x64 applications → Android on Arm · **Status:** Active · **Type:** Android app bundling Wine and Box86/Box64

Winlator packages Wine, Box86/Box64, and graphics drivers into an Android app for running Windows games and utilities. It works best on Snapdragon/Adreno devices and with older or simpler titles. Expect thermal limits, driver-specific bugs, no kernel anti-cheat support, and a non-trivial setup process.

[Source](https://github.com/brunodev85/winlator)

### Hangover

**Route:** Windows x86/x64 applications → Arm64 Linux · **Status:** Active · **Type:** Wine WoW64 with ARM64EC and FEX

Hangover runs x86 and x86-64 Windows applications on Arm64 Wine using ARM64EC and emulator DLLs. Only the application code is emulated; Windows and Unix calls execute natively outside the emulator. It also runs native Arm64 Windows binaries. It is Arm64 Linux only, DXVK needs a Vulkan 1.3 driver, and the project is explicitly experimental.

[Source](https://github.com/AndreRH/hangover)

### Darling

**Route:** macOS applications → Linux · **Status:** Experimental · **Type:** Darwin/macOS compatibility layer

Darling implements macOS's Mach IPC, POSIX, and Darwin syscall interfaces on top of Linux, together with reimplementations of Apple frameworks. It uses DPREFIXes in the spirit of Wine prefixes. Command-line tools are the most usable target; GUI support, AppKit, and the Metal-on-Vulkan backend are still in development. It does not support `.mpkg` installers yet, and prefixes require overlayfs.

[Source](https://github.com/darlinghq/darling)

### touchHLE

**Route:** Early iOS apps (iPhone OS 2.x–4.0) → Windows, macOS, Android, and Linux builds · **Status:** Active · **Type:** High-level emulator

touchHLE does not emulate iPhone hardware. It takes the place of iOS and provides its own Foundation, UIKit, OpenGL ES, and OpenAL implementations, while Dynarmic executes the 32-bit app code. It targets early games rather than modern apps. Compatibility is tracked in a community database, but most apps from the era still do not work, and 64-bit iOS is explicitly out of scope.

[Source](https://github.com/touchHLE/touchHLE) · [Compatibility database](https://appdb.touchhle.org/)

### WSL

**Route:** Linux x86/x64 and arm64 → Windows 10/11 · **Status:** Mature · **Type:** Lightweight virtual machine with OS integration

Windows Subsystem for Linux runs a real Linux kernel in a lightweight VM and integrates filesystem, networking, and process launching with Windows. WSLg adds GUI application support. It is the default way to run Linux tooling on Windows, but it is not a full desktop replacement and does not make arbitrary Linux kernel modules or hardware passthrough work.

[Source](https://github.com/microsoft/WSL) · [Documentation](https://learn.microsoft.com/windows/wsl/)

### Waydroid

**Route:** Android applications → Linux (Arm and x86) · **Status:** Active · **Type:** Container-based Android runtime

Waydroid boots a full Android system image in a Linux container using namespaces, sharing the host kernel. It is the common route for Android apps on Linux phones and desktops. It needs a compatible kernel with binder support, does not ship Google Play services, and running Arm apps on x86 usually requires a native bridge such as libhoudini or libndk.

[Source](https://github.com/waydroid/waydroid)

### libhybris

**Route:** Android hardware libraries → glibc-based Linux distributions · **Status:** Active · **Type:** Low-level compatibility glue

libhybris lets glibc systems load and use bionic-based Android hardware adaptation layers and drivers. It underpins Sailfish OS, Halium, and Ubuntu Touch on Android-derived kernels. It is infrastructure rather than a user-facing emulator: it is distro-specific, low-level, and only useful when the host kernel and hardware match.

[Source](https://github.com/libhybris/libhybris)

### redroid

**Route:** Android → Linux hosts using Docker, Podman, or Kubernetes · **Status:** Active · **Type:** Multi-arch Android container

redroid runs Android in a container on arm64 and amd64 Linux hosts, with GPU support and an optional native bridge for Arm apps on x86. It is aimed at cloud Android and remote-device workloads. It is not a desktop Android emulator, Google Mobile Services are not bundled, and the host kernel must provide the required Android features.

[Source](https://github.com/remote-android/redroid-doc)

## Binary translation and userspace emulation

### FEX

**Route:** x86 and x86-64 Linux binaries → Arm64 Linux · **Status:** Mature · **Type:** Userspace emulator with JIT

FEX runs 32-bit and 64-bit x86 Linux programs on Arm64 Linux and is commonly paired with Wine or Proton for Windows games. It forwards graphics and other API calls to host libraries to reduce overhead. It needs an x86-64 root filesystem and ARMv8.0-A or newer with FEAT_FP and FEAT_CRC32. Performance and compatibility depend heavily on the application and on how well its libraries are thunked.

[Source](https://github.com/FEX-Emu/FEX)

### Box64

**Route:** x86-64 Linux binaries → Arm64, RISC-V 64, and LoongArch Linux · **Status:** Mature · **Type:** Userspace emulator with dynamic recompiler

Box64 translates x86-64 Linux programs on non-x86 64-bit little-endian hosts and wraps native libraries such as libc, SDL, OpenGL, and Vulkan. Its DynaRec is typically 5–10 times faster than interpretation. It supports Wine64 and Proton; 32-bit Windows components need Box86 or Box32. Unity games and OpenGL 3+ titles can still be difficult on low-end Arm or RISC-V boards.

[Source](https://github.com/ptitSeb/box64) · [Compatibility list](https://box86.org/app/)

### Box86

**Route:** x86 Linux binaries → 32-bit Arm Linux (and 64-bit hosts with a 32-bit userland) · **Status:** Active · **Type:** Userspace emulator with dynamic recompiler

Box86 is the 32-bit sibling of Box64. It needs a 32-bit subsystem and toolchain, so it is often used on multiarch Arm systems and alongside Box64 for 32-bit Windows components. It can wrap native libraries for speed. On Switchroot Linux, the lack of Armhf NVIDIA GPU drivers makes accelerated 32-bit games difficult.

[Source](https://github.com/ptitSeb/box86)

### Rosetta 2

**Route:** x86-64 macOS applications → Apple silicon macOS · **Status:** Mature, sunset announced · **Type:** Apple binary translator

Rosetta 2 translates x86-64 macOS applications for Apple silicon, using ahead-of-time translation and a JIT path. It does not translate kernel extensions or virtual-machine apps that virtualize x86-64 platforms, and it does not support AVX-512. Apple now says full Rosetta support ends with macOS 27; from macOS 28 it will retain only a subset for older unmaintained games. Plan around that deadline.

[Documentation](https://developer.apple.com/documentation/apple-silicon/about-the-rosetta-translation-environment) · [2026 support notice](https://developer.apple.com/news/?id=w5ngl9k2) · [Support article](https://support.apple.com/en-us/102527)

### Windows on Arm x86/x64 emulation

**Route:** x86 and x64 Windows applications → Windows 11 on Arm · **Status:** Mature · **Type:** Built-in OS emulation

Windows 11 on Arm emulates x86 and x64 user-mode applications transparently; the improved emulator shipped in recent releases is commonly called Prism. It requires no extra component and is the default route for legacy Windows apps on Snapdragon and other Arm PCs. Kernel drivers, anti-cheat, some instruction-set extensions, and performance-sensitive workloads remain problematic.

[Documentation](https://learn.microsoft.com/windows/arm/apps-on-arm-x86-emulation)

### blink

**Route:** x86-64 Linux binaries → Linux, macOS, and other POSIX hosts · **Status:** Active · **Type:** Minimal userspace emulator

blink is a very small x86-64-linux userspace emulator with a JIT and an interpreter, designed as a simpler alternative to `qemu-x86_64`. It runs Linux binaries, including full chroots, and supports debugging and tracing. It implements the Linux ABI rather than full-system hardware, and some Linux APIs (for example System V message queues) are unsupported.

[Source](https://github.com/jart/blink)

### iSH

**Route:** x86 Linux userland → iOS and iPadOS · **Status:** Active · **Type:** Userspace x86 emulation with syscall translation

iSH runs an Alpine Linux shell on iOS by emulating x86 and translating Linux system calls. Its interpreter uses threaded code rather than JIT, which is why it can ship on the App Store. It is useful for shell tools and small Linux utilities, but it is 32-bit x86 only, cannot use GPU acceleration, and is slow for heavy workloads.

[Source](https://github.com/ish-app/ish) · [App Store](https://apps.apple.com/app/ish-shell/id1436902243)

## Graphics API translation

### DXVK

**Route:** Direct3D 8/9/10/11 → Vulkan · **Status:** Mature · **Type:** Graphics API translation layer

DXVK translates Direct3D 8, 9, 10, and 11 to Vulkan and is a core part of Proton. It supports state caching, graphics pipeline libraries, and a HUD, and it also works on Windows. It requires a reasonably modern Vulkan driver; Direct3D 12 needs VKD3D-Proton. Driver bugs and anti-cheat are the usual blockers.

[Source](https://github.com/doitsujin/dxvk) · [Driver support wiki](https://github.com/doitsujin/dxvk/wiki/Driver-support)

### VKD3D-Proton

**Route:** Direct3D 12 → Vulkan · **Status:** Mature · **Type:** Graphics API translation layer

VKD3D-Proton implements the full Direct3D 12 API on Vulkan and is Proton's D3D12 backend. It aggressively uses modern Vulkan features for performance and game compatibility. It requires Vulkan 1.3 and strong descriptor-indexing support, and it is not a drop-in replacement for the standalone vkd3d API.

[Source](https://github.com/HansKristian-Work/vkd3d-proton)

### MoltenVK

**Route:** Vulkan → Metal on macOS, iOS, tvOS, and visionOS · **Status:** Legacy/deprecated · **Type:** Vulkan portability implementation

MoltenVK layers Vulkan 1.4 over Metal and converts SPIR-V shaders to Metal Shading Language. It remains widely used by emulators and cross-platform engines. Khronos has deprecated it and moved the repository to best-effort community maintenance; it is still the recommended option for Intel Macs and iOS, but KosmicKrisp is now preferred on Apple silicon Macs.

[Source](https://github.com/KhronosGroup/MoltenVK)

### KosmicKrisp

**Route:** Vulkan → Metal 4 on Apple silicon macOS 26+ · **Status:** Active · **Type:** Mesa Vulkan driver

KosmicKrisp is LunarG's Mesa-based Vulkan driver that targets Metal on Apple silicon. It is conformant to Vulkan 1.4, is included with the LunarG Vulkan SDK, and is the recommended Vulkan path for DXVK and VKD3D-Proton on modern Apple silicon. It does not support Intel Macs, requires macOS 26+ and Metal 4, and its iOS build is not yet enabled in the Mesa tree.

[Documentation](https://docs.mesa3d.org/drivers/kosmickrisp.html) · [Vulkan SDK](https://vulkan.lunarg.com/)

### ANGLE

**Route:** OpenGL ES 2.0/3.0/3.1 → Vulkan, Direct3D 9/11, desktop OpenGL, OpenGL ES, Metal, or WebGPU · **Status:** Mature · **Type:** Graphics API translation layer

ANGLE translates OpenGL ES calls to a host graphics API. It is the default WebGL backend in Chrome and Firefox on Windows and is used across browsers, emulators, and applications. It implements OpenGL ES, not the full desktop OpenGL API, and feature support varies by backend.

[Source](https://github.com/google/angle)

### Zink

**Route:** OpenGL → Vulkan · **Status:** Active · **Type:** Mesa Gallium driver

Zink is a Mesa Gallium driver that emits Vulkan calls instead of targeting a GPU directly. It provides desktop OpenGL on devices that only expose Vulkan, and it is useful on Apple silicon through a Vulkan-on-Metal driver. The achievable OpenGL version depends on the underlying Vulkan driver's features, and performance is usually lower than a native OpenGL driver.

[Documentation](https://docs.mesa3d.org/drivers/zink.html)

### Game Porting Toolkit

**Route:** Windows games → macOS on Apple silicon (evaluation); Windows game code → native Mac ports · **Status:** Proprietary · **Type:** Apple porting toolkit

Apple's Game Porting Toolkit combines D3DMetal (Direct3D 9/10/11/12 to Metal), a Metal shader converter, profiling tools, and a Wine-based evaluation environment for running Windows game binaries. It is intended to estimate performance and validate shaders before producing a native port, not as a shipping Windows compatibility layer. Results vary by title, and anti-cheat and launchers remain obstacles.

[Documentation](https://developer.apple.com/games/game-porting-toolkit/)

## Virtual machines and full-system emulation

### QEMU

**Route:** Any supported guest architecture → many host operating systems; full-system and Linux user-mode · **Status:** Mature · **Type:** Emulator and virtual machine monitor

QEMU can emulate a complete machine or run Linux user-mode binaries for another architecture. With KVM, HVF, or another hypervisor it accelerates same-architecture guests to near-native speed; without acceleration it uses TCG dynamic translation. Full-system performance and device emulation vary, and user-mode QEMU only covers the Linux ABI.

[Website](https://www.qemu.org/) · [Source](https://github.com/qemu/qemu)

### UTM

**Route:** Windows, Linux, and other operating systems → macOS, iOS, iPadOS, and visionOS · **Status:** Active · **Type:** QEMU frontend and virtual-machine host

UTM wraps QEMU and Apple's Virtualization framework in a native app. It supports more than 30 processor types, uses QEMU TCG for cross-architecture guests, and can boot macOS guests with Virtualization.framework on Apple silicon. On iOS, JIT requires a jailbreak or a workaround; UTM SE uses an interpreter and is slower but can be sideloaded on unmodified devices.

[Source](https://github.com/utmapp/UTM) · [Website](https://getutm.app/)

### VirtualBox

**Route:** x86/x64 guests → Windows, Linux, and Intel macOS; Arm guests → macOS on Apple silicon and Windows on Arm (7.2+) · **Status:** Mature · **Type:** Hypervisor

VirtualBox is a general-purpose desktop hypervisor. Version 7.2 added macOS on Apple silicon and Windows on Arm as hosts, including Windows 11 Arm guests. It does not officially emulate x86 guests on Arm hosts; use UTM or QEMU for that. Graphics acceleration and guest additions vary by host and guest combination.

[Website](https://www.virtualbox.org/) · [7.2 announcement](https://blogs.oracle.com/virtualization/oracle-virtualbox-72)

### Lima

**Route:** Linux virtual machines → macOS, Linux, and NetBSD hosts · **Status:** Active · **Type:** Linux VM manager

Lima launches Linux virtual machines with automatic file sharing and port forwarding, in the spirit of WSL2 for macOS. It supports containerd, Docker, Podman, and Kubernetes, and uses QEMU or Apple's Virtualization framework under the hood. It is Linux-guest-only and CLI-first, so it is not a route for Windows or macOS guests.

[Source](https://github.com/lima-vm/lima) · [Website](https://lima-vm.io/)

## Browser and WebAssembly

### v86

**Route:** x86 PC operating systems → web browsers · **Status:** Active · **Type:** x86-to-WebAssembly JIT

v86 emulates an x86-compatible CPU and PC hardware in the browser. It boots 32-bit Linux distributions, DOS, Windows 1.01 through 2000, Android-x86, and other x86 systems. It is a full PC emulator, not a Linux ABI layer. 64-bit kernels are not supported, performance is modest, and there is no GPU passthrough.

[Source](https://github.com/copy/v86) · [Demo](https://copy.sh/v86/)

### CheerpX

**Route:** 32-bit x86 Linux binaries and distributions → web browsers · **Status:** Proprietary · **Type:** x86 virtualization in WebAssembly

CheerpX runs unmodified x86 Linux binaries in a WebAssembly sandbox, implements Linux system calls, and can boot complete distributions. It is designed for secure execution of untrusted code in the browser. Only 32-bit x86 is supported, 3D acceleration is not available, and commercial use requires a license.

[Source](https://github.com/leaningtech/cheerpx-meta) · [Website](https://leaningtech.com/cheerpx/)

### JSLinux

**Route:** x86/x86-64 and RISC-V Linux, Windows 2000, and FreeDOS → web browsers · **Status:** Active · **Type:** Browser-based PC emulator

JSLinux is Fabrice Bellard's collection of emulated systems that run entirely in the browser. It includes x86_64 Alpine Linux with AVX-512 and APX support, 32-bit Alpine with a graphical environment, Windows 2000, FreeDOS, and RISC-V Linux images. It is a technical demonstration and lightweight sandbox, not a general-purpose desktop replacement.

[Website](https://bellard.org/jslinux/)

### Wasmtime

**Route:** WebAssembly modules → Windows, Linux, and macOS; embeddable in applications · **Status:** Mature · **Type:** WebAssembly runtime

Wasmtime is a fast, secure, standards-compliant WebAssembly runtime from the Bytecode Alliance. It supports WASI and can run WebAssembly workloads outside the browser or embed them in a host application. It runs WebAssembly, not x86 or operating-system binaries directly; you need a WebAssembly-compiled guest or a runtime such as v86 compiled to Wasm.

[Source](https://github.com/bytecodealliance/wasmtime) · [Website](https://wasmtime.dev/)

## Game console and computer emulators

### Dolphin

**Route:** GameCube and Wii → Windows, Linux, macOS, and recent Android · **Status:** Mature · **Type:** Console emulator

Dolphin emulates the GameCube and Wii with JIT recompilers for x86-64 and Arm64, and supports Direct3D 11/12, OpenGL, Vulkan, and Metal backends. It is one of the most mature console emulators and includes enhancements such as higher resolutions and widescreen. Android performance depends heavily on the GPU driver; there is no official iOS build.

[Source](https://github.com/dolphin-emu/dolphin) · [Website](https://dolphin-emu.org/)

### PCSX2

**Route:** PlayStation 2 → Windows, Linux, and macOS · **Status:** Mature · **Type:** Console emulator

PCSX2 has more than 20 years of development and runs most PS2 games at full speed, with more than 2,500 titles tested in its compatibility database. It requires a PS2 BIOS dump and offers Vulkan, Direct3D 12, and OpenGL backends. Per-game settings are often needed; check the current macOS requirements before installing.

[Source](https://github.com/PCSX2/pcsx2) · [Website](https://pcsx2.net/) · [Compatibility](https://pcsx2.net/compat/)

### RPCS3

**Route:** PlayStation 3 → Windows, Linux, macOS, and FreeBSD · **Status:** Mature · **Type:** Console emulator and debugger

RPCS3 is the leading open-source PS3 emulator, with PPU and SPU recompilers and a Vulkan renderer. It is CPU-intensive and compatibility varies by title, but the compatibility list is extensive. It requires PS3 firmware and legally obtained game dumps.

[Source](https://github.com/RPCS3/rpcs3) · [Website](https://rpcs3.net/)

### shadPS4

**Route:** PlayStation 4 → Windows, Linux, and macOS on Apple silicon · **Status:** Active · **Type:** Console emulator

shadPS4 is an early PS4 emulator written in C++. It executes the PS4's x86-64 code and uses Vulkan for graphics, with a community compatibility list. Many games still fail to boot or render correctly. The macOS build requires macOS 26.0 or later and does not support Intel Macs.

[Source](https://github.com/shadps4-emu/shadPS4) · [Compatibility](https://github.com/shadps4-compatibility/shadps4-game-compatibility)

### KytyPS5

**Route:** PlayStation 5 → Windows and Linux; macOS experimentally through Rosetta 2 and MoltenVK · **Status:** Experimental · **Type:** PS5 translation layer

KytyPS5 is a free and open-source PS5 translation layer derived from Kyty. It executes native PS5 code and targets Vulkan 1.3, with Windows and Linux as the primary platforms. Some 2D and Unreal/Unity titles reach menus or gameplay, but most of the library does not run well. The macOS build is x86-64 and runs under Rosetta 2 on Apple silicon.

[Source](https://github.com/KytyPS5/KytyPS5) · [Website](https://kytyps5.github.io/)

### SharpEmu

**Route:** PlayStation 5 → Windows, Linux, and macOS on x86-64 (Rosetta 2 on Apple silicon) · **Status:** Experimental · **Type:** PS5 emulator

SharpEmu is an experimental PS5 emulator written from scratch in C#. It can load `eboot.bin`, execute native CPU instructions, and partially handle GPU work, with Vulkan on Windows/Linux and MoltenVK on macOS. Compatibility is limited and the project is focused on accuracy and infrastructure rather than finished games.

[Source](https://github.com/sharpemu/sharpemu) · [Compatibility](https://sharpemu.app/compatibility/)

### xemu

**Route:** Original Xbox → Windows, Linux, and macOS · **Status:** Active · **Type:** Console emulator

xemu emulates the original Xbox, including its NVIDIA GPU and Pentium III CPU. It requires an Xbox BIOS/MCPX boot ROM and game dumps, and supports Vulkan and OpenGL rendering. It does not run Xbox 360 software; use XenonRecomp-based ports for that.

[Source](https://github.com/xemu-project/xemu) · [Website](https://xemu.app/)

### PPSSPP

**Route:** PlayStation Portable → Windows, macOS, Linux, Android, and iOS · **Status:** Mature · **Type:** Console emulator

PPSSPP is a high-level PSP emulator that does not require a BIOS. It supports Vulkan, OpenGL, and Direct3D 11, and has official Arm64 Windows builds. Compatibility is good for most titles, and the project maintains a community compatibility database. iOS builds have JIT and App Store restrictions.

[Source](https://github.com/hrydgard/ppsspp) · [Website](https://www.ppsspp.org/) · [Compatibility](https://report.ppsspp.org/games)

### melonDS

**Route:** Nintendo DS → Windows, Linux, macOS, and Android · **Status:** Active · **Type:** Console emulator

melonDS emulates the Nintendo DS and DSi, with an OpenGL renderer and support for direct game boot or firmware boot. Firmware boot requires a BIOS/firmware dump from an original DS or DS Lite; DSi and 3DS firmware dumps are only suitable for direct boot. Some DSi features and Wi-Fi behavior remain incomplete.

[Source](https://github.com/melonDS-emu/melonDS) · [Website](https://melonds.kuribo64.net/)

### mGBA

**Route:** Game Boy, Game Boy Color, and Game Boy Advance → Windows, macOS, Linux, Android, Switch, 3DS, Vita, Wii, and PS5 through RetroArch · **Status:** Mature · **Type:** Console emulator

mGBA is an accurate GBA emulator that also supports GB and GBC games. It includes link cable support, real-time clock, solar sensor, Game Boy Camera/Printer, e-Reader, Lua scripting, and GDB debugging. A few mappers are only partially supported. It is the recommended GBA core for RetroArch, which is how GBA games reach the Switch and PS5.

[Source](https://github.com/mgba-emu/mgba) · [Website](https://mgba.io/) · [Downloads](https://mgba.io/downloads.html)

### DOSBox-X

**Route:** DOS, Windows 3.x/9x/ME, and NEC PC-98 software → Windows, Linux, and macOS · **Status:** Active · **Type:** PC emulator

DOSBox-X is a DOSBox fork focused on accurate emulation and configurability. It runs DOS games and applications, Windows 3.x/9x/ME, DOS/V, and PC-98 software, and exposes many hardware and configuration options. It is not a general Windows application runner, and accurate emulation is CPU-intensive.

[Source](https://github.com/joncampbell123/dosbox-x) · [Website](https://dosbox-x.com/)

### Cemu

**Route:** Wii U → Windows, Linux, and macOS (experimental) · **Status:** Active · **Type:** Console emulator

Cemu emulates the Wii U and supports Vulkan and OpenGL rendering. The native macOS build is experimental and is known to have performance issues due to MoltenVK and Rosetta on Apple silicon. It requires legally dumped game files and keys, and online functionality is not a priority.

[Source](https://github.com/cemu-project/Cemu) · [Website](https://cemu.info/)

### RetroArch

**Route:** Many classic consoles and computers → Windows, Linux, macOS, Android, iOS, Switch, PS5 homebrew, and more · **Status:** Mature · **Type:** libretro frontend

RetroArch is the main frontend for the libretro API, with cores for NES, SNES, GB/GBC/GBA, DS, N64, PlayStation, PS2, PSP, arcade, DOS, and other systems. It provides shaders, netplay, rewind, and a consistent controller interface. Core quality varies, some cores need BIOS/firmware, and iOS App Store builds are constrained by JIT rules. The PS5 port is an alpha homebrew project.

[Source](https://github.com/libretro/RetroArch) · [Website](https://www.retroarch.com/) · [Cores](https://docs.libretro.com/)

### Ryujinx

**Route:** Nintendo Switch 1 → Windows, Linux, and macOS · **Status:** Legacy (community fork active) · **Type:** Console emulator

Ryujinx is a C# Nintendo Switch emulator known for accuracy. The original project was discontinued in October 2024; the Ryubing community fork continues at `git.ryujinx.app`, with Vulkan, OpenGL, and Metal (through MoltenVK) rendering and a compatibility list of roughly 4,300 tested titles. It requires Switch firmware and keys dumped from your own console. There is no official Android build.

[Website](https://ryujinx.app/) · [Source (Ryubing fork)](https://git.ryujinx.app/projects/Ryubing)

### Eden

**Route:** Nintendo Switch 1 → Windows, Linux, macOS, and Android · **Status:** Active · **Type:** Console emulator

Eden is a community Switch emulator derived from Yuzu. It is developed on its own Forgejo instance after Nintendo DMCA takedowns removed the project from GitHub, and it offers Vulkan rendering and Android builds. It requires firmware and keys, compatibility varies by title, and there is no iOS build.

[Website](https://eden-emu.dev/) · [Source](https://git.eden-emu.dev/eden-emu/eden)

## Game engine reimplementations

### ScummVM

**Route:** Classic adventure games and supported engines → Windows, macOS, Linux, Android, iOS, and consoles · **Status:** Mature · **Type:** Game engine reimplementation

ScummVM reimplements the engines of hundreds of adventure games, including SCUMM, Sierra, and other classic engines, and runs the original game data. It is not an emulator and does not include games. Compatibility is tracked per title, and only supported games work.

[Source](https://github.com/scummvm/scummvm) · [Website](https://www.scummvm.org/) · [Compatibility](https://www.scummvm.org/compatibility/)

### OpenMW

**Route:** The Elder Scrolls III: Morrowind → Windows, macOS, and Linux · **Status:** Mature · **Type:** Game engine reimplementation

OpenMW is an open-source reimplementation of Morrowind's engine. It loads the original game data, adds improved graphics and UI, and supports mods, but mod compatibility is not perfect because OpenMW's script compiler is stricter than the original. It requires a legitimate Morrowind installation and does not support 32-bit platforms.

[Source](https://github.com/OpenMW/openmw) · [Website](https://openmw.org/)

## Static recompilation and native ports

### N64Recomp

**Route:** Nintendo 64 binaries → C code → native executables on any platform · **Status:** Active · **Type:** Static recompiler

N64Recomp translates N64 machine code into C that can be compiled for modern platforms. It supports relocatable overlays and RSP microcode, and it is the foundation of Zelda 64: Recompiled. It is a tool for producing ports, not a general-purpose emulator: each game needs metadata, analysis, and often runtime work.

[Source](https://github.com/N64Recomp/N64Recomp)

### Zelda64Recomp

**Route:** The Legend of Zelda: Majora's Mask (and Ocarina of Time in progress) → Windows, Linux, and macOS · **Status:** Active · **Type:** Static recompilation port

Zelda 64: Recompiled uses N64Recomp to turn Majora's Mask into a native port with high frame rates, widescreen support, and mods through the RT64 renderer. It requires the North American ROM and a GPU with Direct3D 12, Vulkan 1.2, or Metal Argument Buffers Tier 2. Ocarina of Time support is in development.

[Source](https://github.com/Zelda64Recomp/Zelda64Recomp) · [Website](https://zelda64.dev/)

### XenonRecomp

**Route:** Xbox 360 executables → C++ → native executables · **Status:** Active · **Type:** Static recompiler

XenonRecomp converts Xbox 360 PowerPC executables into C++ that can be compiled for modern platforms. Many VMX instructions are implemented with x86 intrinsics or SIMDe, and setjmp/longjmp are redirected to native implementations. MMIO and exceptions are not fully supported, so each game needs additional reverse engineering.

[Source](https://github.com/hedge-dev/XenonRecomp)

### UnleashedRecomp

**Route:** Sonic Unleashed (Xbox 360) → Windows and Linux · **Status:** Active · **Type:** Static recompilation port

Unleashed Recompiled is an unofficial PC port of the Xbox 360 version of Sonic Unleashed, built with XenonRecomp and XenosRecomp. It adds high frame rates, ultrawide support, improved performance, and modding. It requires US or EU game files; the Japanese version is not supported, and the CPU must support AVX.

[Source](https://github.com/hedge-dev/UnleashedRecomp)

## Console-as-host routes

### nxnative

**Route:** Nintendo Switch 1 games → iPhone and iPad (native execution, not emulation) · **Status:** Experimental · **Type:** Native porting toolkit

nxnative rewrites Switch NSO binaries, stands in for the Horizon OS, and translates NVN graphics to Metal, producing a sideloadable IPA from an NSP folder and `prod.keys`. It requires iOS/iPadOS 26 or later and an A17 Pro or M1-or-newer device because the games use BC-compressed textures. Only a small set of titles is supported, and it does not include any game data.

[Source](https://github.com/seedds/nxnative)

### ProsperoEden

**Route:** Nintendo Switch 1 games → jailbroken PlayStation 5 · **Status:** Experimental · **Type:** PS5 port of the Eden emulator

ProsperoEden is an unofficial PS5 port of Eden, packaged as native PS5 homebrew. It uses a Vulkan renderer running on a PS5 Mesa/RADV port, and requires a jailbreak plus your own firmware, keys, and game files. Early reports show lighter titles running around 40 FPS while heavier games struggle. It is alpha software and does not work with PSN.

[Source](https://github.com/Swordpdf/ProsperoEden)

### PS5 RetroArch

**Route:** GBA, DS, PS1, PS2, PSP, and other classic systems → jailbroken PlayStation 5 · **Status:** Experimental · **Type:** Native RetroArch homebrew

PS5 RetroArch is a native RetroArch homebrew title for jailbroken PS5 consoles, with native Vulkan rendering, fifteen release cores, and a local WebUI. The core list includes mGBA for Game Boy/GBC/GBA and LRPS2 for PlayStation 2. It is an alpha release, requires a jailbreak and your own content/BIOS/firmware, and does not run PS5 games.

[Source](https://github.com/mihawk-99/PS5_RetroArch) · [Mirror](https://github.com/SocialMDev/ps5_retroarch)

### ps5-linux

**Route:** Linux → PlayStation 5 Phat, Slim, and Pro on firmware 3.00–13.60 · **Status:** Experimental · **Type:** Hypervisor exploit and Linux bootloader

ps5-linux uses patched hypervisor vulnerabilities to boot a custom Linux image on a PS5, turning the console into a Linux PC with eight cores, sixteen threads, and access to the GPU. It supports USB and M.2 installation, and can run Steam games and emulators. It requires a jailbroken console on a specific firmware, cannot be downgraded, and sacrifices PSN access. The lead developer stepped away from the scene in 2026, so treat long-term support as uncertain.

[Source](https://github.com/ps5-linux/ps5-linux-loader)

### Lakka

**Route:** RetroArch/Linux → Nintendo Switch 1 (Switchroot L4T Lakka beta), PCs, and single-board computers · **Status:** Active · **Type:** RetroArch-based Linux distribution

Lakka is a lightweight LibreELEC-based distribution built around RetroArch. On Switch 1 it is installed through Switchroot and hekate on an unpatched console, and it is one of the ways to turn the console into a RetroArch host. Switchroot also offers Android, L4T Ubuntu, and L4T Fedora. These routes are community projects: they require an unpatched console, can be fragile, and are not supported by Nintendo.

[Website](https://www.lakka.tv/) · [Switch guide](https://www.lakka.tv/doc/Nintendo-Switch/) · [Switchroot FAQ](https://wiki.switchroot.org/wiki/faq)

## Route recipes

### Windows games on Apple silicon

1. Install Rosetta 2 if prompted.
2. Use **CrossOver** for the simplest supported path, or the **Game Porting Toolkit** evaluation environment for testing.
3. Let CrossOver/GPTK choose D3DMetal, or pair Wine with **DXVK**/**VKD3D-Proton** on top of **KosmicKrisp** (Apple silicon, macOS 26+) or **MoltenVK** (Intel Macs and older setups).
4. Expect anti-cheat, launchers, and media codecs to be the failure points. This is not a universal Windows compatibility layer.

### Windows apps on Arm Linux

- Use **FEX** with Wine/Proton for x86 and x86-64 Windows binaries.
- Use **Hangover** when you want Wine's WoW64 with ARM64EC and FEX emulator DLLs.
- Use **Box64** with Wine64 for x86-64 binaries and **Box86**/Box32 for 32-bit components.
- Keep expectations realistic: Vulkan 1.3 drivers, thunked libraries, and per-app fixes matter more than raw CPU speed.

### Windows apps on Android

- Install **Winlator** and let it create a container with Wine and Box86/Box64.
- Prefer older or simpler games; Snapdragon/Adreno devices have the best driver support.
- There is no general solution for kernel anti-cheat, and thermal throttling is normal.

### Linux on Windows

- Use **WSL2** for command-line tools, development, and GUI apps through WSLg.
- Use **VirtualBox** or another hypervisor for a full desktop Linux VM.
- WSL2 is not a full desktop replacement; GPU compute and nested virtualization depend on the Windows build and hardware.

### Linux on a Mac

- Use **UTM** or **Lima** for arm64 Linux. On Apple silicon, arm64 guests can run near-native through Apple's Virtualization framework.
- Use **UTM** with QEMU TCG for x86 Linux guests, or **VirtualBox** only for supported Arm guests.
- For x86 Linux binaries without a full VM, try **iSH** on iOS/iPadOS (32-bit userland) or **QEMU** user-mode on macOS.

### Android on Linux

- Use **Waydroid** for a desktop Android experience on a Linux host.
- Use **redroid** for containerized Android instances on servers or CI.
- Both need a compatible kernel and do not include Google Play services by default.

### Switch 1 games

- On Windows, Linux, and macOS: **Ryujinx** (Ryubing fork) or **Eden**.
- On Android: **Eden** (or Ryujinx where an Android build is available).
- On iPhone/iPad: **nxnative** for a small number of titles, with native execution rather than emulation.
- On a jailbroken PS5: **ProsperoEden**.
- You need your own console keys, firmware, and game dumps. The projects do not supply them.

### Make a Switch 1 or PS5 into a host

- **Switch 1:** install Switchroot (Android, L4T Ubuntu/Fedora, or L4T Lakka) on an unpatched console. Use **Lakka** or **RetroArch** for emulation, and Box64 + Wine only for light Windows software.
- **PS5:** boot **ps5-linux** on a supported firmware, then run Steam/Proton and emulators. Alternatively stay in the PS5 OS and use **PS5 RetroArch** and **ProsperoEden** homebrew.
- Both routes require jailbreaking or homebrew, can break online access, and are not endorsed by the vendors.

### GBA everywhere

- Desktop, Android, and most handhelds: **mGBA** directly, or **RetroArch** with the mGBA core.
- iPhone/iPad: **RetroArch** with the mGBA core.
- Switch 1: **mGBA** or **RetroArch** homebrew.
- PS5: **PS5 RetroArch** with the mGBA core.
- Browser: use a WebAssembly GBA emulator; none is included in this list yet.

### Xbox and Xbox 360

- Original Xbox: **xemu** with a BIOS/MCPX boot ROM and game dumps.
- Xbox 360: there is no general-purpose emulator in this list. **XenonRecomp** and **UnleashedRecomp** show the static-recompilation route for specific titles.

### Old iOS games

- Use **touchHLE** for 32-bit apps from iPhone OS 2.x–4.0.
- Check the compatibility database first; most apps do not work.
- There is no practical route for modern 64-bit iOS apps on non-Apple hosts.

## Legal and practical notes

- **No content is included.** This list does not contain ROMs, ISOs, BIOS files, console firmware, decryption keys, or game data. You must dump and use content you own, where your local law allows it.
- **Emulators are not the same as piracy.** Many emulators are legal, but distributing or downloading copyrighted games, firmware, and keys is not. The legality of dumping and format-shifting varies by jurisdiction.
- **Jailbreaking has consequences.** It can void warranties, block online services, and put accounts at risk. PS5 Linux and homebrew routes may be incompatible with PSN, and firmware updates can remove the exploit.
- **Anti-cheat and DRM are out of scope.** This list does not cover bypassing kernel anti-cheat, DRM, or account restrictions.
- **Performance is workload-specific.** A route that works for a 2D indie game may fail for a AAA title with anti-cheat, and a fast CPU does not compensate for a missing GPU feature.
- **Upstream projects change.** Status labels reflect the primary sources checked on 2026-10-24. MoltenVK is deprecated in favour of KosmicKrisp on Apple silicon, Apple plans to end full Rosetta support after macOS 27, and the Switch emulator scene has moved to community forks and Forgejo. Open an issue if a route has changed.


## Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

In short:

- One project per pull request, with a primary source and a concrete guest → host route.
- Add the entry to the right category and update the matrix if the route is new.
- State the status and the main limitation; do not describe an experimental project as mature.
- Update both `README.md` and `README.zh-CN.md`.
- Do not add piracy tools, ROM/firmware links, or anti-cheat bypasses.
- Run the link checker or confirm that the new links resolve.

---

Licensed under the [MIT License](LICENSE). Project names and trademarks belong to their respective owners.
