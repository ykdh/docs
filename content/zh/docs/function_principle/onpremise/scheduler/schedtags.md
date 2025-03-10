---
title: "策略调度"
date: 2022-04-11T20:50:28+08:00
weight: 100
description: >
  介绍调度标签、调度策略功能概念以及举例如何实现调度功能。
---

Cloudpods基于调度标签实现复杂的调度策略。

## 调度标签

调度标签用于与资源进行绑定，从而实现资源的调度。调度标签支持绑定到宿主机、网络、块存储等基础资源。管理员可根据资源使用场景及用途为为其创建并静态绑定标签或创建动态调度。

调度标签的默认策略，包括尽量使用、避免使用、禁止使用、无，请注意必须使用不可以设置为调度标签的默认策略。当创建虚拟机时如果没有匹配到任何调度策略，也没有指定任意调度标签的偏好，则默认策略生效。

- 尽量使用（prefer）：绑定这类调度标签的资源的默认策略为尽量使用。即尽量使用绑定这类调度标签的资源，如果不带这类标签，也可以使用。
- 避免使用（avoid）：绑定这类调度标签的资源的默认策略为避免使用。即尽量避免使用绑定这类调度标签的资源，如果带这种标签，也可以使用。
- 禁止使用（exclude）：绑定这类调度标签的资源的默认策略为禁止使用。即禁止使用绑定这类调度标签的资源。
- 无：绑定这类调度标签的资源的默认策略为无。

## 动态调度标签

动态调度标签即调度标签与资源绑定的一种方式，根据设定的条件在资源调度前动态为宿主机绑定调度标签，每次调度宿主机绑定的标签不一定相同，从而实现资源的灵活调度。

## 调度策略

设置在满足指定条件时，将会根据偏好选择或排除在绑定某一类调度标签的宿主机创建虚拟机。调度策略中的偏好将会覆盖调度标签中的默认偏好。调度策略将会影响最后的调度结果。

调度策略中偏好分为以下四种：

- 尽量使用（prefer）：调度时优先使用拥有这种调度标签的宿主机。如没有带这种调度标签的宿主机、也可以接受。
- 避免使用（avoid）：调度时尽量避免拥有这种调度标签的宿主机，如果有带这种调度标签的宿主机、也可以接受。
- 禁止使用（exclude）：调度时必须排除拥有这种调度标签的宿主机。
- 必须使用（require）：调度时必须使用拥有这种调度标签的宿主机。

## 举例说明

以宿主机为例介绍，介绍调度标签以及调度策略如何实现资源调度功能。

### 调度策略的应用

如有一批宿主机只给特定的项目A使用，其他项目不能用这些宿主机。可通过以下操作实现调度功能。

1. 创建一个 owner_by_project 的调度标签，默认策略为 禁止使用，将这个标签绑定到对应的宿主机。
2. 创建调度策略，偏好为尽量使用调度标签 owner_by_project ，条件则选择具体的项目A。
3. 后续用户在项目A中创建虚拟机时，调度策略保持默认，则系统将会根据调度策略选择带owner_by_project 调度标签的宿主机创建虚拟机。在其它项目中创建虚拟机时则不会选择带owner_by_project 调度标签的宿主机。

### 动态调度标签的应用


当宿主机空闲的 cpu 个数小于 10 或者 memory 小于 2g 时，尽量避免调度虚拟机到这些宿主机。

1. 创建调度标签low_cpu_mem，默认策略为 避免使用。
2. 创建动态调度标签 host_low_cpu_mem，添加匹配条件为 'host.free_cpu_count < 10 || host.free_mem_size < 2048'，对应 low_cpu_mem 调度标签。目前界面暂不支持设置CPU内存的条件，可通过Climc命令进行设置。

```
# 创建调度标签
$ climc schedtag-create low_cpu_mem --strategy avoid
# 创建动态调度标签机制
$ climc dynamic-schedtag-create --enable host_low_cpu_mem low_cpu_mem 'host.free_cpu_count < 10 || host.free_mem_size < 2048'
```
