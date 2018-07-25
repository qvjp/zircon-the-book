# Fuchsia不是Linux
_一个模块化,高性能操作系统_

[English](https://fuchsia.googlesource.com/docs/+/master/the-book/)

本文档是Fuchsia操作系统描述文章集合，围绕特定子系统组织，文档将持续完成。

## Zircon 内核

Zircon是Fuchsia下边剩余部分的微内核. Zircon提供了核心驱动和Fuchsia的libc实现.

 - [Concepts][zircon-concepts]
 - [System Calls][zircon-syscalls] / VDSO (libzircon)

## Zircon核心

 - Device Manager & Device Hosts
 - [Device Driver Model (DDK)][zircon-ddk]
 - [C Library (libc)](libc.md)
 - [POSIX I/O (libfdio)](life_of_an_open.md)
 - [Process Creation](process_creation.md)

## 框架

 - [Core Libraries](core_libraries.md)
 - Application model
   - [Interface definition language (FIDL)][FIDL]
   - Services
   - Environments
 - [Boot sequence](boot_sequence.md)
 - Device, user, and story runners
 - Components
 - [Namespaces](namespaces.md)
 - [Sandboxing](sandboxing.md)
 - [Story][framework-story]
 - [Module][framework-module]
 - [Agent][framework-agent]
 - Links

## 存储

 - [Block devices](block_devices.md)
 - [File systems](filesystems.md)
 - Directory hierarchy
 - [Ledger][ledger]
 - Document store
 - Application cache

## 网络

 - Ethernet
 - [Wireless](wireless_networking.md)
 - [Bluetooth][bluetooth]
 - Sockets
 - HTTP

## 图形

 - [Magma (vulkan driver)][magma]
 - [Escher (physically-based renderer)][escher]
 - [Scenic (compositor)][scenic]
 - [Input manager][input-manager]
 - [View manager][view-manager]
 - [Flutter (UI toolkit)][flutter]

## 媒体

 - Audio
 - Video
 - DRM

## Intelligence

 - Context
 - Agent Framework
 - Suggestions

## 用户界面

 - Device, user, and story shells
 - Stories and modules

## 向后兼容性

 - POSIX lite (what subset of POSIX we support and why)
 - Web runtime

## 更新和恢复

 - Verified boot
 - Updater

[zircon-concepts]: concepts-zh.md
[zircon-syscalls]: https://fuchsia.googlesource.com/zircon/+/master/docs/syscalls.md
[zircon-ddk]: https://fuchsia.googlesource.com/zircon/+/HEAD/docs/ddk/overview.md
[FIDL]: https://fuchsia.googlesource.com/zircon/+/HEAD/docs/fidl/index.md
[framework-story]: https://fuchsia.googlesource.com/peridot/+/master/docs/modular/story.md
[framework-module]: https://fuchsia.googlesource.com/peridot/+/master/docs/modular/module.md
[framework-agent]: https://fuchsia.googlesource.com/peridot/+/master/docs/modular/agent.md
[ledger]: https://fuchsia.googlesource.com/peridot/+/master/docs/ledger/README.md
[bluetooth]: https://fuchsia.googlesource.com/garnet/+/HEAD/bin/bluetooth/README.md
[magma]: https://fuchsia.googlesource.com/garnet/+/master/lib/magma/
[escher]: https://fuchsia.googlesource.com/garnet/+/master/public/lib/escher/
[scenic]: https://fuchsia.googlesource.com/garnet/+/master/docs/ui_scenic.md
[input-manager]: https://fuchsia.googlesource.com/garnet/+/master/docs/ui_input.md
[view-manager]: https://fuchsia.googlesource.com/garnet/+/master/bin/ui/view_manager/
[flutter]: https://flutter.io/
