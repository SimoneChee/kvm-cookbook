# Chapter 2: 从零搭建 KVM 环境

> 一台干净的 Linux 服务器，到能跑虚拟机，只需要这几步。

## 2.1 环境要求

| 项目 | 要求 |
|------|------|
| CPU | 支持 VT-x（Intel）/ AMD-V（AMD） |
| 内存 | ≥ 4GB（生产建议 64GB+） |
| 磁盘 | ≥ 50GB（虚拟机镜像占用） |
| 系统 | CentOS 7+ / Ubuntu 20.04+ / openEuler |

## 2.2 检查 CPU 虚拟化支持

```bash
# Intel 输出 vmx，AMD 输出 svm
grep -E '(vmx|svm)' /proc/cpuinfo | head -1
```

> 如果没有任何输出，需要进 BIOS 开启 VT-x / AMD-V。

## 2.3 安装 KVM 与相关工具

### CentOS / RHEL

```bash
yum install -y qemu-kvm libvirt virt-install bridge-utils
yum install -y virt-manager   # 可选：图形管理
```

### Ubuntu / Debian

```bash
apt update
apt install -y qemu-kvm libvirt-daemon-system libvirt-clients \
               virtinst bridge-utils
```

### openEuler

```bash
dnf install -y qemu-kvm libvirt virt-install
```

## 2.4 启动并设置开机自启

```bash
systemctl enable --now libvirtd
systemctl status libvirtd
```

## 2.5 验证环境

```bash
# 全面检查
virt-host-validate

# 查看 KVM 模块
lsmod | grep kvm

# 查看虚拟网络
virsh net-list --all
```

预期输出：

```
 QEMU: Checking for hardware virtualization : PASS
 QEMU: Checking for device /dev/kvm         : PASS
 QEMU: Checking for device /dev/vhost-net   : PASS
```

## 2.6 配置网络

默认安装会创建 NAT 网桥 `virbr0`（192.168.122.0/24）。生产环境建议创建独立的 bridge 供虚拟机使用：

```bash
# 创建网桥 br0 并绑定物理网卡
nmcli con add type bridge ifname br0
nmcli con add type ethernet slave-type bridge con-name bridge-br0 ifname eth0 master br0
nmcli con up br0
```

> 网桥的详细配置（bond、VLAN 等）见第二部分「网络篇」。

## 2.7 常见问题

| 问题 | 解决 |
|------|------|
| `/dev/kvm` 不存在 | 检查 BIOS 虚拟化、`modprobe kvm_intel` |
| libvirtd 启动失败 | `journalctl -u libvirtd -n 50` 查看日志 |
| 权限不足 | 将用户加入 `libvirt` 组：`usermod -aG libvirt $USER` |

---

**下一章**：[Chapter 3: 创建第一个虚拟机](03-first-vm.md)
