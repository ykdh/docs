---
title: "Redis实例"
weight: 2
description: Redis是一种支持key-value等多种数据结构的非关系型数据库。
---

Redis是一种支持key-value等多种数据结构的存储系统，具有以下特点：

- 支持内存加硬盘的混合存储方式；
- 支持字符串（String）、哈希（Hash）、列表（List）、集合结构（Set、Sorted Set）、流（Stream）等数据结构。

目前{{<oem_name>}}平台已支持对接并创建阿里云、华为云、腾讯云的Redis实例，AWS、Azure的Redis实例以及腾讯云平台的云缓存Memcached暂只支持同步，其他平台正在陆续支持中。

**使用流程**

1. 创建Redis实例，具体内容请参考[新建Redis实例](#新建Redis实例)。
2. 如需控制连接Redis数据库的IP地址，请设置白名单，具体内容请参考[新建白名单](#新建白名单)。
3. 系统创建后只有管理员账号，用户可根据需求创建账号并设置权限，具体内容请参考[新建账号](#新建账号)。
4. 使用账号连接Redis数据库。

**入口**：在云管平台单击左上角![](../../images/intro/nav.png)导航菜单，在弹出的左侧菜单栏中单击 **_"数据库/Redis/Redis实例"_** 菜单项，进入Redis实例页面。

![](../../images/database/redis.png)

- 列表右上方将显示Redis实例总数以及处于运行、关机、操作失败以及未知状态的Redis实例数量。

    ![](../../../images/computing/statistics.png)

## 新建Redis实例

该功能用于新建Redis实例，目前仅支持创建阿里云、华为云、腾讯云的Redis实例。

1. 在Redis实例页面，单击列表上方 **_"新建"_** 按钮，进入新建Redis实例页面。
2. 配置以下参数：
    - 名称：Redis实例的名称。
    - 指定项目：管理员或域管理员在新建Redis实例时需要指定Redis实例所属项目。
    - 计费方式：包括按量付费、包年包月。
        - 按量付费：按Redis实例的实际使用量付费，此模式适用于设备需求量会瞬间大幅度增大的场景，价格对比包年包月要贵。
        - 包年包月：是一种预付费的模式，提前一次性支付一个月、一年乃至多年的费用，该模式适用于设备需求比较平稳的场景，价格相对按量付费更便宜。选择包年包月后还需要设置购买时长。
    - 到期释放：设置新建的Redis实例的使用时长，超过设置时间后的Redis实例将会被删除。仅按量付费的Redis实例支持到期释放。
    - 数量：设置创建的Redis实例的数量。
    - 区域：选择Redis实例所在地域，并在对应地域内选择平台、区域和可用区。不同地域支持的平台情况可能不同，请以实际界面为准。
    - 类型：即Redis。
    - 版本：Redis的版本信息。
        - 阿里云、腾讯云支持Redis 2.8、4、5；
        - 华为云支持Redis 4、5。
    - 实例类型：
        - 基础版：仅支持单副本。
        - 高可用：仅支持双副本。
        - 集群：阿里云、腾讯云集群支持单副本和双副本；华为云集群仅支持双副本。
        - 读写分离（仅阿里云支持）：Redis读写分离版本由代理服务器（Proxy Servers）、主备（Master and Replica）节点及只读（Read-Only）节点组成。
    - 节点类型：主要包括单副本、双副本、只读节点。
        - 单副本：单副本为单节点缓存架构，没有数据可靠性保障。
        - 双副本：双副本为一主一从的双机热备架构，数据持久化保存；
        - 只读节点：主节点写，从节点只读。支持选择只读节点（1个）、只读节点（3个）、只读节点（5个）。
    - 性能类型：支持标准性能和增强性能
    - 内存：根据内存可过滤出可用套餐。
    - 套餐：显示Redis实例的规格、平台、节点类型、备可用区、内存、CPU、存储架构、分片数、最大连接数、价格等信息。
    - 管理员密码：支持随机生成和手工输入管理员密码。
    - VPC：选择VPC和IP子网。
    - 安全组：选择安全组，限制实例的安全访问规则，最多可绑定5个安全组。仅腾讯云支持。
3. 单击 **_"确定"_** 按钮，完成操作。

## 同步状态

该功能用于获取Redis实例的当前状态。

**单个同步状态**

1. 在Redis实例列表，单击Redis实例右侧操作列 **_"同步状态"_** 按钮，同步Redis实例状态。

**批量同步状态**

1. 在Redis实例列表中选择一个或多个Redis实例，单击列表上方 **_"批量操作"_** 按钮，选择下拉菜单 **_"同步状态"_** 菜单项，批量同步Redis实例状态。

## 重启

该功能用于重启Redis实例，重启Redis实例会造成连接中断，请谨慎操作。

1. 根据需要重启的Redis实例数量，选择重启方式。
    - 重启单个Redis实例：单击Redis实例右侧操作列 **_更多_** 按钮，选择下拉菜单 **_"重启"_** 菜单项，在弹出的操作确认对话框中单击 **_"确定"_** 按钮。
    - 批量重启：在Redis实例列表中选择一个或多个Redis实例，单击列表上方 **_批量操作_** 按钮，选择下拉菜单 **_"重启"_** 菜单项，在弹出的操作确认对话框中单击 **_"确定"_** 按钮。

## 调整配置

该功能用于调整Redis实例的配置信息，实例配置只能向上调整。腾讯云暂不支持调整配置。

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"调整配置"_** 菜单项，弹出调整配置对话框。
2. 支持调整存储类型、CPU核数、内存、存储空间等参数。
3. 配置调整完成后，单击 **_"确定"_** 按钮。

## 清空数据

该功能用于清除Redis实例中的数据，数据清空后将无法再找回，请谨慎操作。

{{% alert title="说明" %}}
腾讯云非免密Redis实例清空数据时需要输入管理员密码。
{{% /alert %}}

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"清空数据"_** 菜单项，弹出操作确认对话框。
2. 单击 **_"确定"_** 按钮，完成操作。

## 重置密码

该功能用于修改Redis实例的密码。

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"重置密码"_** 菜单项，弹出修改密码对话框。
2. 设置账户密码为随机生成或手动输入，若选择手动输入，需要设置密码。
3. 设置完成后，单击 **_"确定"_** 按钮。 

## 更改项目

该功能用于更改Redis实例的所属项目。

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"更改项目"_** 菜单项，弹出修改项目对话框。
2. 选择域和项目，单击 **_"确定"_** 按钮，完成操作。


## 关联安全组

安全组是一种虚拟的包过滤防火墙，通过设置安全组规则来控制Redis实例的访问规则等。仅腾讯云平台的Redis实例支持关联安全组。

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"关联安全组"_** 菜单项，弹出关联安全组对话框。
2. 在关联安全组对话框中支持关联或取消关联安全组。
    - 关联安全组：选择要绑定的安全组，最多支持5个。如没有符合需求的安全组，可单击“新建安全组”超链接，在弹出的新建安全组页面配置相关参数，单击 “确定” 按钮，创建安全组。
    - 取消关联安全组：取消选择安全组，至少保留一个安全组。
3. 单击 **_"确定"_** 按钮，完成操作。

## 开启/关闭免密访问

仅支持在内网环境下开启免密访问，开启免密访问后，同一VPC内的虚拟机可以不使用密码访问Redis实例，也可以继续使用用户名和密码的方式访问Redis实例。

{{% alert title="说明" %}}
- 阿里云、腾讯云平台Redis实例支持开启和关闭免密访问。
- 外网连接不支持设置免密访问。
{{% /alert %}}

### 开启免密访问

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"开启免密访问"_** 菜单项，弹出操作确认对话框。
2. 单击 **_"确定"_** 按钮，开启免密访问。

### 关闭免密访问

开启免密访问后，原来开启免密访问菜单项将变成关闭免密访问。关闭免密访问之后请同步修改连接Redis实例的应用程序的认证方式等。

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"关闭免密访问"_** 菜单项，弹出操作确认对话框。
2. （腾讯云）重置用户密码，单击 **_"确定"_** 按钮，关闭免密访问。
3. （其他云）单击 **_"确定"_** 按钮，关闭免密访问。

## 到期释放

到期释放即设置Redis实例的使用时长，当超过设置期限时，Redis实例将会被自动删除。

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"到期释放"_** 菜单项，弹出到期释放对话框。
2. 选择是否勾选到期释放，勾选后设置释放时间，单击 **_"确定"_** 按钮。
    - 单击释放时间输入框，显示日历等内容，支持直接在释放时间输入框中输入日期和时间，格式为yyyy-mm-dd hh:mm:ss，单击 **_"确定"_** 按钮。 
    - 在日历框中选择日期，单击 **_选择时间_** 按钮，将跳转到设置时间界面，选择时间后，单击 **_"确定"_** 按钮。
    - 支持快速选择“1小时”、“2小时”、“3小时”、 “6小时”、 “1天”、“2天”、 “1周”按钮，释放时间将设置为对应时间范围内的时间，单击 **_"确定"_** 按钮。
3. 设置完成后，在计费方式列显示Redis实例还有多久时间释放等信息。
4. 若不再需要到期释放功能，可在释放之前，在弹出的到期释放对话框中取消勾选到期释放，单击 **_"确定"_** 按钮。

## 续费

该功能用于为将有过期的包年包月的Redis实例进行续费操作。

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"续费"_** 菜单项，弹出续费菜单项。
2. 选择续费时长，单击 **_"确定"_** 按钮，完成续费操作。

## 自动续费设置

该功能用于为包年包月的Redis实例进行自动续费操作。

{{% alert title="说明" %}}
阿里云平台按周购买的虚拟机续费周期是一周，按月购买的虚拟机续费周期是一个月，按年购买的虚拟机续费周期是一年；其他平台续费周期均为一个月。后续虚拟机在每次到期之前都会自动续费。
{{% /alert %}}

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"自动续费设置"_** 菜单项，弹出自动续费设置对话框。
2. 自动续费：勾选自动续费，单击 **_"确定"_** 按钮，设置自动续费。
3. 取消自动续费：取消勾选自动续费，单击 **_"确定"_** 按钮，取消自动续费。

## 设置删除保护

该功能用于设置Redis实例的删除保护。当Redis实例启用删除保护后，Redis实例无法被删除；当Redis实例禁用删除保护后，Redis实例才可以被删除。

**单个Redis实例设置删除保护**

1. 禁用删除保护：
    - 单击Redis实例名称右侧带有![](../../images/computing/delprotect1.png)图标时，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"设置删除保护"_** 菜单项，弹出设置删除保护对话框。
    - 选择"禁用"删除保护，单击 **_"确定"_** 按钮。
2. 启用删除保护：
    - 当Redis实例名称右侧不带![](../../images/computing/delprotect1.png)图标时，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"设置删除保护"_** 菜单项，弹出设置删除保护对话框。
    - 选择"启用"删除保护，单击 **_"确定"_** 按钮。

**批量设置删除保护**

1. 禁用删除保护：
    - 在Redis实例列表中勾选一个或多个Redis实例，单击列表上方 **_"批量操作"_** 按钮，选择下拉菜单 **_"设置删除保护"_** 菜单项，弹出设置删除保护对话框。
    - 选择"禁用"删除保护，单击 **_"确定"_** 按钮，批量为Redis实例禁用删除保护。
2. 启用删除保护：
    - 在Redis实例列表中勾选一个或多个Redis实例，单击列表上方 **_"批量操作"_** 按钮，选择下拉菜单 **_"设置删除保护"_** 菜单项，弹出设置删除保护对话框。
    - 选择"启用"删除保护，单击 **_"确定"_** 按钮，批量为Redis实例启用删除保护。

## 删除

该功能用于删除Redis实例，当Redis实例名称项右侧有![](../../images/computing/delprotect1.png)图标，表示Redis实例启用了删除保护，无法删除Redis实例，如需删除Redis实例，需要先禁用删除保护。

**单个删除**

1. 在Redis实例页面，单击Redis实例右侧操作列 **_"更多"_** 按钮，选择下拉菜单 **_"删除"_** 菜单项，弹出操作确认对话框。
2. 单击 **_"确定"_** 按钮，完成操作。

**批量删除**

1. 在Redis实例列表中选择一个或多个Redis实例，单击列表上方 **_"删除"_** 按钮，弹出操作确认对话框。
2. 单击 **_"确定"_** 按钮，完成操作。

## 查看Redis实例详情

该功能用于查看Redis实例的详细信息。

1. 单击Redis实例名称项，进入Redis实例详情页面。
2. 查看以下信息：
    - 基本信息：包括云上ID、ID、名称、状态、域、项目、平台、计费方式、区域、可用区（若有备可用区，也将在此处显示）、云账号、创建时间、更新时间、备注。
    - 数据库信息：包括类型版本、节点类型、性能类型、可维护时间段、实例规格、CPU、储存架构、内存。
    - 链接信息：包括内网地址、外网地址以及开启外网地址操作（仅阿里云支持）、数据库端口号、VPC、子网、访问方式、安全组。
        - 开启外网地址仅阿里云Redis实例支持，开启后需要等待一分钟所有才会开启成功，开启成功后用户可通过外网地址和端口后连接数据库。
    - 其他信息：支持开启或关闭删除保护。

## 白名单设置

为了保证Redis数据库的安全性，用户可以将访问Redis实例的IP地址和IP地址段添加到对应实例的白名单中，仅在白名单上的IP地址被允许访问Redis实例。不设置白名单默认允许所有IP访问Redis实例。

目前仅阿里云平台的Redis实例支持白名单功能。

### 新建白名单

该功能用于新建白名单。

1. 在Redis实例详情页面，单击“白名单设置”页签，弹出新建白名单对话框。
2. 设置分组名，在白名单中输入允许访问Redis实例的IP地址和IP地址段，单击 **_"确定"_** 按钮。
3. 白名单创建完成后，只有在实例白名单中的IP地址才可以访问Redis实例。

{{% alert title="说明" %}}
- 当有多个IP地址和IP地址段时，需要用英文逗号分隔。
- 一个实例最多支持设置20个IP地址和IP地址段。
{{% /alert %}}

### 修改白名单

该功能用于修改白名单。

1. 在Redis实例详情页面，单击“白名单设置”页签，进入白名单页面。
2. 单击白名单右侧操作列 **_"修改"_** 按钮，弹出修改白名单对话框。
3. 支持修改分组名和允许访问Redis实例的IP地址和IP地址段，单击 **_"确定"_** 按钮。

### 删除白名单

该功能用于删除白名单。

1. 在Redis实例详情页面，单击“白名单设置”页签，进入白名单页面。
2. 单击白名单右侧操作列 **_"删除"_** 按钮，弹出操作确认对话框。
3. 单击 **_"确定"_** 按钮，完成操作。


## 账号管理

支持为Redis实例创建多个账号并分配不同权限，从而更加灵活的管理Redis实例。

Redis实例创建完成后，默认会创建一个管理员账号，其中华为云Redis实例上管理员账号不支持任何操作，阿里云edis实例上管理员账号支持重置密码操作。

目前仅阿里云Redis实例支持账号管理功能，且要求阿里云实例的引擎版本为Redis 4.0及以上版本。

### 新建账号

1. 在Redis实例详情页面，单击“账号管理”页签，进入账号管理页面。
2. 单击列表上方 **_"新建"_** 按钮，弹出新建账号对话框。
3. 设置以下参数。
    - 账户名称：用于访问Redis实例的账号名称。
    - 权限设置：设置账号的权限，权限包括只读、读写、复制。
    - 密码：设置账号的密码。
    - 确认密码：再次输入密码。
4. 单击 **_"确定"_** 按钮，完成操作。

### 重置密码

该功能用于重新设置账号密码，当用户忘记账号密码时，可以重新设置密码。

1. 在Redis实例详情页面，单击“账号管理”页签，进入账号管理页面。
2. 单击账号右侧操作列 **_"重置密码"_** 按钮，弹出重置密码对话框。
3. 设置新密码，确认密码，单击 **_"确定"_** 按钮，完成操作。

### 修改权限

该功能用于修改用户对于Redis实例的权限。管理员账号不支持修改权限操作。

1. 在Redis实例详情页面，单击“账号管理”页签，进入账号管理页面。
2. 单击账号右侧操作列 **_"修改权限"_** 按钮，弹出修改权限对话框。
3. 设置账号权限，修改完成后，单击 **_"确定"_** 按钮。

### 删除

该功能用于删除Redis实例下的账户。管理员账号不支持删除。

1. 在Redis实例详情页面，单击“账号管理”页签，进入账号管理页面。
2. 单击账号右侧操作列 **_"删除"_** 按钮，弹出操作确认对话框。
3. 单击 **_"确定"_** 按钮，完成操作。

## 备份列表

为了保障数据安全，提高系统可靠性，Redis支持备份还原操作备份Redis实例数据，当数据损坏或丢失后，用户可使用备份还原数据。

华为云基础版Redis实例不支持备份和还原操作。

### 新建备份

该功能用于立即创建备份数据。备份时要求实例处于运行中状态。

1. 在Redis实例详情页面，单击“备份列表”页签，进入备份页面。
2. 单击列表上方 **_"新建"_** 按钮，弹出新建备份对话框。
3. 设置名称、描述，单击 **_"确定"_** 按钮，立即备份Redis实例。

### 恢复备份

该功能用于将Redis实例恢复到备份时状态。腾讯云不支持恢复还原操作

1. 在Redis实例详情页面，单击“备份列表”页签，进入备份页面。
2. 单击备份右侧操作列 **_"恢复"_** 按钮，弹出操作确认对话框。
3. 单击 **_"确定"_** 按钮，完成操作。

## 查看Redis实例监控

该功能用于查看Redis实例的监听数据。

1. 在Redis实例详情页面，单击“监控”页签，进入监控页面。
2. 查看以下监控信息。
    - 支持查看近1小时、近1天、近1周、近1月、近3月、近6月的性能监控信息，支持设置时间粒度，时间粒度大小与查看的时长有关。
    - 支持查看的监控指标如下表。

<style> 
table tr th, table tr td  { border:1px solid #808080; padding:5px; }
</style>

平台 | 支持查看的指标项 
---------|----------
 阿里云 - Redis | CPU使用率、内存使用率、网络瞬间输入流量、网络瞬间输出流量、活跃客户端数量、每秒并发操作数、缓存键总数、已过期的键的总数、数据集使用内存
 华为云 - Redis |  CPU使用率、内存使用率、网络瞬间输入流量、网络瞬间输出流量、活跃客户端数量、每秒并发操作数、缓存键总数、已过期的键的总数、数据集使用内存
 腾讯云 - Redis | CPU使用率、内存使用率、网络瞬间输入流量、网络瞬间输出流量、活跃客户端数量、每秒并发操作数、缓存键总数、已过期的键的总数、数据集使用内存
 Azure - Redis | CPU使用率、内存使用率、CPU负载率、活跃客户端数量、连接错误数



## 查看Redis实例操作日志

该功能用于查看Redis实例相关操作的日志信息。

1. 在Redis实例详情页面，单击“操作日志”页签，进入操作日志页面。
    - 加载更多日志：列表默认显示20条操作日志信息，如需查看更多操作日志，请单击 **_"加载更多"_** 按钮，获取更多日志信息。
    - 查看日志详情：单击操作日志右侧操作列 **_"查看"_** 按钮，查看日志的详情信息。支持复制详情内容。
    - 查看指定时间段的日志：如需查看某个时间段的操作日志，在列表右上方的开始日期和结束日期中设置具体的日期，查询指定时间段的日志信息。
    - 导出日志：目前仅支持导出本页显示的日志。单击右上角![](../../images/system/download.png)图标，在弹出的导出数据对话框中，设置导出数据列，单击 **_"确定"_** 按钮，导出日志。
