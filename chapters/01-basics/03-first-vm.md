# Chapter 3: 创建第一个虚拟机

> 两种方式：命令行 `virt-install`（推荐，可脚本化）和 XML 定义（精细控制）。

## 3.1 准备安装镜像

```bash
# 下载 CentOS 7 最小化镜像（约 1GB）
wget http://mirrors.aliyun.com/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-2009.iso \
  -P /var/lib/libvirt/images/
```

## 3.2 方式一：virt-install 快速创建

```bash
virt-install \
  --name my-first-vm \
  --memory 4096 \
  --vcpus 4 \
  --disk path=/var/lib/libvirt/images/my-first-vm.qcow2,size=50,format=qcow2 \
  --os-variant centos7 \
  --network bridge=virbr0,model=virtio \
  --cdrom /var/lib/libvirt/images/CentOS-7-x86_64-Minimal-2009.iso \
  --graphics vnc,listen=0.0.0.0 \
  --noautoconsole
```

参数说明：

| 参数 | 说明 |
|------|------|
| `--memory` | 内存（MB），生产建议 ≥4096 |
| `--vcpus` | vCPU 数量 |
| `--disk` | 磁盘：qcow2 格式、50GB 动态分配 |
| `--os-variant` | 操作系统类型，影响默认设备配置 |
| `--network` | 网络：bridge 模式 + virtio 驱动（性能最佳） |
| `--graphics` | 图形：VNC 用于安装过程 |

## 3.3 方式二：XML 定义创建

先用 `virt-install --print-xml` 生成模板，或手写 domain XML：

```xml
<domain type='kvm'>
  <name>my-first-vm</name>
  <memory unit='MiB'>4096</memory>
  <vcpu placement='static'>4</vcpu>
  <os>
    <type arch='x86_64' machine='pc-q35-rhel8.0.0'>hvm</type>
    <boot dev='cdrom'/>
  </os>
  <devices>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source file='/var/lib/libvirt/images/my-first-vm.qcow2'/>
      <target dev='vda' bus='virtio'/>
    </disk>
    <interface type='bridge'>
      <source bridge='virbr0'/>
      <model type='virtio'/>
    </interface>
  </devices>
</domain>
```

```bash
# 用 XML 定义并启动
virsh define my-first-vm.xml
virsh start my-first-vm
```

## 3.4 连接安装界面

```bash
# 查看 VNC 端口
virsh vncdisplay my-first-vm
# 输出类似 :0（即 5900 端口）

# 通过 VNC 客户端连接 host:5900 完成系统安装
```

## 3.5 验证虚拟机

```bash
# 查看状态
virsh list --all

# 查看详细信息
virsh dominfo my-first-vm

# 进入控制台
virsh console my-first-vm
```

## 3.6 关键优化项（新手容易忽略）

| 优化 | 说明 |
|------|------|
| `model=virtio` | 磁盘和网卡都用 virtio，性能比默认 e1000/ide 高数倍 |
| `qcow2` 格式 | 动态分配，节省空间；支持快照 |
| `--os-variant` 正确 | 影响 CPU 模型和默认设备，务必指定 |

## 3.7 小结

- `virt-install` 适合快速创建和脚本化
- XML 定义适合精细控制和版本管理
- virtio 驱动是性能关键，务必使用

---

**下一篇**：[Chapter 4: Bridge vs OVS vs Macvtap](../02-network/04-bridge-vs-ovs.md)
