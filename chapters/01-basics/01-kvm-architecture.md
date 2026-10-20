# Chapter 1: KVM 架构与原理

> 理解 KVM 的架构，是掌握虚拟化技术的第一块基石。

## 1.1 什么是 KVM

KVM（Kernel-based Virtual Machine）是 Linux 内核的一个虚拟化模块，它让 Linux 内核本身成为一个 Hypervisor。

```
┌─────────────────────────────────────────────────────────────┐
│                    虚拟机 1        虚拟机 2        虚拟机 3  │
│                  ┌─────────┐   ┌─────────┐   ┌─────────┐   │
│  用户空间        │  App    │   │  App    │   │  App    │   │
│                  │  GuestOS│   │  GuestOS│   │  GuestOS│   │
│                  └────┬────┘   └────┬────┘   └────┬────┘   │
├───────────────────────┼─────────────┼─────────────┼─────────┤
│                       │   QEMU      │   QEMU      │  QEMU   │
│  用户空间             │ (设备模拟)   │ (设备模拟)   │(设备模拟)│
├───────────────────────┼─────────────┼─────────────┼─────────┤
│                       ▼             ▼             ▼         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              KVM 内核模块 (/dev/kvm)                 │   │
│  │    ┌──────────┐  ┌──────────┐  ┌──────────┐        │   │
│  │    │ vCPU 0   │  │ vCPU 1   │  │ vCPU n   │        │   │
│  │    └──────────┘  └──────────┘  └──────────┘        │   │
│  └─────────────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────┤
│                    Linux 内核 + 硬件 (CPU 支持 VT-x/AMD-V)  │
└─────────────────────────────────────────────────────────────┘
```

## 1.2 核心组件关系

KVM 虚拟化栈由三个关键组件组成：

### QEMU（Quick EMUlator）
- **作用**：负责设备模拟（磁盘、网卡、显示器等）
- **本质**：用户空间进程，每个虚拟机对应一个 QEMU 进程
- **关键点**：通过 `/dev/kvm` 与内核 KVM 模块交互

### KVM 内核模块
- **作用**：负责 CPU 和内存的虚拟化
- **本质**：加载后使 Linux 内核具备 Hypervisor 能力
- **关键点**：利用 Intel VT-x 或 AMD-V 硬件虚拟化扩展

### Libvirt
- **作用**：虚拟化管理层，提供统一的 API
- **本质**：守护进程 `libvirtd` + 客户端工具（virsh、virt-manager）
- **关键点**：屏蔽底层差异，支持 KVM、Xen、LXC 等多种 Hypervisor

## 1.3 三者如何协作

一次 VM 启动的完整流程：

```bash
# 1. 用户通过 virsh 发起启动命令
virsh start my-vm

# 2. virsh 连接 libvirtd（通过 unix socket）
#    libvirtd 解析 domain XML 定义

# 3. libvirtd 启动 QEMU 进程
#    qemu-system-x86_64 -name my-vm -cpu host -m 4096 ...

# 4. QEMU 打开 /dev/kvm 设备

# 5. KVM 模块为 VM 创建 vCPU 和内存映射

# 6. QEMU 进入 KVM 运行模式，VM 开始执行
```

## 1.4 验证环境

检查你的系统是否支持 KVM：

```bash
# 检查 CPU 是否支持虚拟化
grep -E '(vmx|svm)' /proc/cpuinfo

# 检查 KVM 模块是否加载
lsmod | grep kvm

# 检查 /dev/kvm 设备
ls -l /dev/kvm

# 使用 virt-host-validate 全面检查
virt-host-validate
```

输出示例：

```
  QEMU: Checking for hardware virtualization                 : PASS
  QEMU: Checking for device /dev/kvm                         : PASS
  QEMU: Checking for device /dev/vhost-net                   : PASS
  ...
```

## 1.5 关键概念

| 概念 | 说明 | 类比 |
|------|------|------|
| Domain | 一台虚拟机的完整定义 | 进程 |
| Hypervisor | 虚拟化层，管理所有 VM | 操作系统 |
| vCPU | 虚拟 CPU，映射到物理核 | 线程 |
| Guest | 运行在 VM 内的操作系统 | 程序 |
| Host | 运行 Hypervisor 的物理机 | 宿主机 |

## 1.6 小结

- KVM 是**内核级**虚拟化，性能接近原生
- QEMU + KVM 是黄金组合：QEMU 管设备，KVM 管 CPU/内存
- Libvirt 提供了统一的管理接口，是生产环境的标准选择

---

**下一章**：[Chapter 2: 从零搭建 KVM 环境](02-environment-setup.md)
