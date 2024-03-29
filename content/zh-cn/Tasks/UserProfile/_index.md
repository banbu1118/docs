---
title: "使用FSLogix管理用户配置"
linkTitle: "使用FSLogix管理用户配置"
date: 2022-08-14
type: "docs"
weight: 6
description: >
   本文介绍了使用 FSLogix 在 DoraCloud 桌面云环境中增强和启用用户配置文件的过程。
---

### 1.介绍

在桌面云办公的使用场景中，经常会遇到如下需求：

-  **镜像更新** ：需要对用户的桌面进行统一更新。比如系统进行更新，或者安装新的应用。 

-  **桌面修复** ：用户的桌面系统出现故障，管理员希望在不破坏用户数据的情况下快速修复桌面系统。

对于需求1，如果是非持久化桌面，可以通过编辑桌面模板镜像后，重新创建用户的桌面来实现。 但是如果用户在桌面中保存有用户配置，那么管理员更新镜像后，无法对用户的桌面系统进行重建。因为重建用户桌面会丢失用户配置数据。

对于需求2，如果非持久桌化桌面，管理员可以通过还原桌面，或者重建桌面，快速恢复桌面系统。 对于持久化桌面，删除现有桌面重新创建会导致用户配置丢失。

FSLogix是一套用户配置管理的解决方案。它支持配置文件容器，可以将用户的配置保存在桌面系统盘之外的位置。 在启用FSLogix管理用户配置文件后，管理员可以像维护非持久化桌面一样维护办公桌面。 管理员可以更新镜像，然后重建桌面，每个用户可以得到新的桌面系统，同时不丢失用户的配置。 在遇到桌面故障时，也可以通过删除重新创建桌面，快速恢复用户的桌面系统。

DoraCloud桌面云使用FSlogix有两种方式。 一种是DoraCloud启用AD认证的情况下，使用文件共享服务来集中保存用户的配置。第二种方式是直接将用户配置保存在用户的数据盘（D盘）。第二种方式不需要AD认证。

本文主要介绍第二种方式的配置。


### 2.安装步骤

2.1. 下载FSlogix

下载地址：  https://aka.ms/fslogix-latest

2.2. 编辑模板，安装 FSlogix

在DoraCloud中，选择桌面模板->编辑，进入模板编辑状态。

进入桌面模板Windows，打开安装包，然后运行FSLogixAppsSetup.exe，即可完成安装。 

{{< figure src="./images/fslogix01.png" width="600">}}


2.3. 【本地模式】配置FSlogix

FSLogix提供了两种配置FSLogix的方法。一种是运行ConfigurationTool.exe；另一种是通过组策略修改。 由于ConfigurationTool修改的参数不全，这里通过组策略修改。

从FSLogix安装包中提取“fslogix.adml”、“fslogix.admx”两个文件。

将  fslogix.adml    文件复制到当前正在编辑的桌面模板的   C:\Windows\PolicyDefinitions\en-US   文件夹中；

将  fslogix.admx    文件复制到当前正在编辑的桌面模板的    C:\Windows\PolicyDefinitions\      文件夹中；

然后启用本地组策略编辑器  gpedit.msc ，找到如下图 FSLogix组策略位置
{{< figure src="./images/fslogix03.png" width="600">}}

依次配置“Enabled”、“VHD Location”、“Dynamic VHD(x) Allocation”、“Delete local profile when FSlogix Profile should apply” 这4个选项。
{{< figure src="./images/fslogix04.png" width="800">}}

配置完毕后，，关闭组策略编辑器，进入cmd命令窗口，执行更新组策略。

```
gpupdate /force
```

这里需要说明一下，由于桌面的数据盘是动态分配的，编辑模板的时候，模板虚拟机没有D盘。 因此FSLogix对于用户配置的重定向是不会生效的。 

为了验证这个FSLogix配置是否有效，可以在虚拟化平台中为模板新增一个D盘，然后重启模板虚拟机进行测试。操作步骤如下：

1、先对模板虚拟机做一个快照。

2、为模板虚拟机增加一个的D盘。

3、重启模板虚拟机，然后登录桌面，检查是否生效。如果FSLogix配置生效，在登录时会提示进行个性化设置。

4、恢复快照，删除快照。

接下来继续模板编辑操作。将模板保存。


### 3.桌面池启用支持 FSLogix的模板

在DoraCloud后台创建桌面池。桌面池启用上一步安装了 FSLogix 的模板。 桌面池启用用户数据盘。 根据需求创建桌面池。 

### 4.测试 FSLogix 的用户配置管理功能

在 DoraCloud 后台创建用户，为用户分配第3步创建的桌面池。然后登录桌面。

 **测试用例1：删除桌面，重新分配桌面，用户数据配置仍然保留。** 

 **测试步骤：** 

1）用户登录桌面。

2）用户修改桌面配置，比如执行如下操作：

- 在桌面创建文件。 
- 在我的文档中创建文件。
- 安装 WPS 应用程序，桌面出现WPS图标。

3）删除用户的桌面。

4）用户重新登录，分配新的桌面，用户的配置不变。 


 **测试用例2：统一更新用户的桌面系统** 

 **测试步骤：** 

1）桌面池已经分配给若干用户在使用。

2）管理原编辑桌面模板，添加新的应用软件。

3）管理员删除用户的桌面，然后系统使用模板的新版本自动创建新的桌面。

4）用户重新登录，分配新的桌面，用户连接桌面windows后，确认用户的配置都还在。

上述测试步骤中，用户自己安装的应用能否被FSLogix配置管理保存，取决于应用的安装程序的处理机制。比如WPS的安装程序会将自己安装到用户的配置文件中。这样桌面删除重新分配时，WPS程序仍然存在。 另外一个应用程序会将程序安装在系统区域中。如果用户安装了这类程序，在桌面重新分配时，这类程序会丢失。




