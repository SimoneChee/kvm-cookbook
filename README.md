# KVM Cookbook 📖 — 虚拟化实战手册

<p align="center">
  <strong>从入门到生产部署的完整 KVM 虚拟化实战指南</strong><br>
  <em>基于 H3C 云平台生产环境经验整理</em>
</p>

<p align="center">
  <a href="https://github.com/SimoneChee/kvm-cookbook/stargazers"><img src="https://img.shields.io/github/stars/SimoneChee/kvm-cookbook?style=social" alt="Stars"></a>
  <a href="https://github.com/SimoneChee/kvm-cookbook/blob/main/LICENSE"><img src="https://img.shields.io/github/license/SimoneChee/kvm-cookbook" alt="License"></a>
</p>

---

## 📚 关于本手册

这是一份**基于真实生产环境**的 KVM 虚拟化实战手册。不同于照本宣科的教材，这里的所有内容都来自维护过程中踩过的坑、总结的经验。

### 为什么写这份手册？

市面上关于 KVM 的资料很多，但大多存在以下问题：
- ❌ **只讲理论**：讲 QEMU/KVM 架构，但不告诉你怎么落地
- ❌ **脱离生产**：示例都是 demo 级别，遇到真实问题就抓瞎
- ❌ **碎片化**：网络、存储、性能各自为战，缺少全局视角
- ❌ **版本过时**：还在讲 2015 年的 libvirt 用法  

本手册力求**实战、系统、前沿**，每个章节都配有可直接执行的命令和脚本。

## 📖 目录

### 第一部分：基础篇

| 章节 | 内容 | 状态 |
|------|------|------|
| [Chapter 1](chapters/01-basics/01-kvm-architecture.md) | KVM 架构与原理 | ✅ 已完成 |
| [Chapter 2](chapters/01-basics/02-environment-setup.md) | 从零搭建 KVM 环境 | ✅ 已完成 |
| [Chapter 3](chapters/01-basics/03-first-vm.md) | 创建第一个虚拟机 | ✅ 已完成 |

### 第二部分：网络篇

| 章节 | 内容 | 状态 |
|------|------|------|
| Chapter 4 | Bridge vs OVS vs Macvtap | 🔄 进行中 |
| Chapter 5 | VLAN 与 VxLAN 网络隔离 | 🔄 进行中 |
| Chapter 6 | Bond 与 LACP 高可用网络 | 📝 规划中 |
| Chapter 7 | SR-IOV 与智能网卡直通 | 📝 规划中 |
| Chapter 8 | netlink 编程实战（Go） | 📝 规划中 |

### 第三部分：存储篇

| 章节 | 内容 | 状态 |
|------|------|------|
| Chapter 9 | 存储池类型与选型 | 📝 规划中 |
| Chapter 10 | iSCSI 多路径配置 | 📝 规划中 |
| Chapter 11 | OCFS2 共享存储集群 | 📝 规划中 |
| Chapter 12 | 磁盘热插拔与在线扩容 | 📝 规划中 |

### 第四部分：高级篇

| 章节 | 内容 | 状态 |
|------|------|------|
| Chapter 13 | NUMA 亲和性与 CPU Pinning | 📝 规划中 |
| Chapter 14 | 实时迁移原理与实践 | 📝 规划中 |
| Chapter 15 | 设备透传与 GPU 虚拟化 | 📝 规划中 |
| Chapter 16 | 安全加固（SELinux/sVirt） | 📝 规划中 |
| Chapter 17 | 性能调优实战 | 📝 规划中 |

### 第五部分：运维篇

| 章节 | 内容 | 状态 |
|------|------|------|
| Chapter 18 | 故障排查方法论 | 📝 规划中 |
| Chapter 19 | 常见问题 FAQ | 📝 规划中 |
| Chapter 20 | 监控与告警体系 | 📝 规划中 |

### 附录

| 附录 | 内容 | 状态 |
|------|------|------|
| Appendix A | virsh 命令速查表 | 📝 规划中 |
| Appendix B | XML 配置模板库 | 📝 规划中 |

## 🚀 快速开始

无需按顺序阅读，根据你的需求直接跳到对应章节：

- **刚接触 KVM？** → 从 [Chapter 1](chapters/01-basics/01-kvm-architecture.md) 开始
- **遇到网络问题？** → 看 [第二部分：网络篇](chapters/02-network/)
- **存储性能差？** → 看 [第三部分：存储篇](chapters/03-storage/)
- **要做性能调优？** → 看 [第四部分：高级篇](chapters/04-advanced/)
- **线上出了故障？** → 看 [第五部分：运维篇](chapters/05-operations/)

## 🤝 贡献

欢迎通过 Issue 和 PR 参与贡献。如果你有真实案例想要分享，请：
1. Fork 本仓库
2. 创建你的分支
3. 提交 PR

## 📄 许可证

[知识共享署名-非商业性使用-相同方式共享 4.0 (CC BY-NC-SA 4.0)](LICENSE)


- GitHub: [@SimoneChee](https://github.com/SimoneChee)
- 专注：KVM 虚拟化、Linux 网络、分布式存储、SR-IOV、华为 Ascend NPU

---

<p align="center">
  <strong>⭐ 觉得有用就点个 Star，你的支持是我更新的动力！</strong>
</p>
