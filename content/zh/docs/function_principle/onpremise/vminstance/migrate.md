---
title: "迁移相关"
weight: 30
description: >
  介绍如何迁移虚拟机，以及迁移虚拟机相关的注意事项。
---

## 介绍

目前迁移功能主要用于 {{<oem_name>}} 内置私有云的虚拟机上，该功能用于将虚拟机迁移到其他宿主机上面。当未指定宿主机时，系统将自动选择宿主机。


迁移的方式分为“冷迁移”和“热迁移”，区别如下：

- 冷迁移：在虚拟机**关机的状态**下，将虚拟机磁盘从源宿主机拷贝到目标宿主机。
- 热迁移：在虚拟机**运行的状态**下，将虚拟机的磁盘以及内存状态同步到目标宿主机，当两边数据同步后，再将虚拟机切换到目标宿主机。

热迁移和冷迁移比起来，能够在不关机，保证业务运行的情况下，将虚拟机从一台宿主机迁移到另一台宿主机。

但热迁移默认要求目标宿主机和源宿主机的 CPU microcode 一致，可以通过以下命令查看 CPU 的 microcode：

```bash
$ cat /proc/cpuinfo | grep microcode | uniq
microcode       : 0xca
```

## Climc

### 冷迁移

虚拟机在关机状态下，可以执行冷迁移操作。

#### 查看冷迁移帮助信息

冷迁移的命令为 `climc server-migrate`，通过下面的命令可以查看参数的说明和帮助信息：

```bash
$ climc server-migrate --help

Usage: climc server-migrate [--auto-start] [--rescue-mode] [--prefer-host PREFER_HOST] <ID>

Migrate server

Positional arguments:
    <ID>
        ID of server

Optional arguments:
    [--auto-start]
        Server auto start after migrate
    [--rescue-mode]
        Migrate server in rescue mode, all disks must reside on shared storage
    [--prefer-host PREFER_HOST]
        Server migration prefer host id or name
```

#### 冷迁移举例

- 对已经关机的虚拟机 vm1 进行迁移

```
# 先查看虚拟机的信息
$ climc server-list --search vm1 --details

# 执行随机迁移
$ climc server-migrate vm1

# 迁移过程中虚拟机会处于 migrating 的状态
$ climc server-list --search vm1

# 迁移完成后虚拟机状态变为 ready，并且可以发现宿主机信息也发生了变化
$ climc server-list --search vm1 --details
```

- 迁移到指定宿主机

```
# 先列出平台的 kvm 宿主机
$ climc host-list --hypervisor kvm

# 然后指定宿主机名称或者 id 迁移
$ climc server-migrate --prefer-host xxx vm1
```

- 迁移后自动启动虚拟机

```
$ climc server-migrate --auto-start vm1

$ climc server-migrate --auto-start --prefer-host xxx vm1
```

- rescue mode 迁移：当宿主机完全宕机，虚拟机所有的磁盘都使用共享存储（ceph 等分布式块存储）的情况下，可以通过 rescue mode 把元数据迁移到另外的宿主机，然后启动

```bash
$ climc server-migrate --rescue-mode \
    --auto-start \
    --prefer-host $host_name \
    $vm_name
```

### 热迁移

虚拟机在运行状态进行的迁移叫做热迁移，热迁移的速度会比冷迁移慢，因为设计磁盘和内存等状态的同步，但好处是在不关机的状态下进行迁移，基本上对虚拟机上运行的业务没有影响。

#### 查看热迁移帮助信息

热迁移的命令为 `climc server-live-migrate`，通过下面的命令查看帮助信息：

```bash
$ climc server-live-migrate --help
Usage: climc server-live-migrate [--skip-cpu-check] [--prefer-host PREFER_HOST] <ID>

Live-Migrate server

Positional arguments:
    <ID>
        ID of server

Optional arguments:
    [--skip-cpu-check]
        Skip check CPU mode of the target host
    [--prefer-host PREFER_HOST]
        Server migration prefer host id or name
```

#### 热迁移举例

- 对虚拟机 vm1 进行热迁移，目标宿主机随机选择

```bash
$ climc server-live-migrate vm1
```

- 热迁移默认要求目标宿主机和虚拟机当前所在的宿主机 CPU microcode 、内核版本等一致， 如果不一致该宿主机则会被调度器过滤掉，如果环境里面实在没有 CPU 一致的目标宿主机，可以使用 `--skip-cpu-check` 绕过 CPU microcode 的检查

```bash
$ climc server-live-migrate --skip-cpu-check vm1
```

- 指定 vm1 热迁移到目标宿主机 host1 ，并且绕过 CPU microcode 检查

```bash
$ climc server-live-migrate \
    --prefer-host host1 \
    --skip-cpu-check \
    vm1
```

### 宿主机宕机自动迁移

#### 原理

宿主机宕机自动迁移会自动检测宿主机的在线状况，当控制器（region）检测到宿主机离线，则会自动将宿主机上使用共享存储的虚拟机在别的宿主机上启动起来。宿主机宕机检测原理是 host会维持 etcd 上的一个 key (路径为：/onecloud/kvm/host/health/<host_id>)，region会watch这个key，一旦host离线，这个key会超时删除，region检测到这个事件后，开始把host上的共享存储主机强制迁移到别的宿主机。

#### 开启宕机自动迁移

通过如下步骤开启宕机自动迁移：

##### 修改宿主机host配置

```bash
vi /etc/yunion/host.conf
把
enable_health_checker: false
改为true
enable_health_checker: true
```

##### 修改region服务的配置

```bash
climc service-config-show region2
climc service-config-edit region2
把
enable_host_health_check: false
改为
enable_host_health_check: true
climc host-auto-migrate-on-host-down --enable  {宿主机ID}
```

##### 重启服务

```bash
kubectl -n onecloud rollout restart daemonset default-host
kubectl -n onecloud rollout restart deployment default-region
```
