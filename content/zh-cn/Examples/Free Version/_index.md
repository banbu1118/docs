---
title: "使用DoraCloud免费版搭建办公桌面云"
linkTitle: "使用DoraCloud免费版搭建办公桌面云"
date: 2022-12-17
type: "docs"
weight: 5
tags: ["DoraCloud","免费版","云办公"]
categories: ["应用案例"]
description: >
  使用DoraCloud免费版搭建办公桌面云
---

DoraCloud是一款多平台的桌面虚拟化管理软件，支持Hyper-V、VMware、Proxmox、XenServer等多种虚拟化平台。DoraCloud在虚拟化平台上具有极大的灵活性，允许您的组织自由选择合适的IT基础设施来构建桌面云；也允许您的组织重用现有的IT设施基础，而不是抛弃现有的IT基础设施。

DoraCloud采用一体化设计理念，把桌面虚拟化所需的组件打包在一个虚拟机镜像中，极大的简化了虚拟桌面部署的难度。无论是单服务器的桌面云系统，还是数十个节点的桌面云集群，都只需要部署DoraCloud虚拟机镜像。

DoraCloud提供免费版，可以支持25个用户。 本文介绍了在Proxmox虚拟化平台上部署DoraCloud免费版 ，在Windows上安装DoraClient客户端程序，实现免费的桌面云办公方案。除了DoraClient客户端，也可以购买朵拉云的云终端，接入DoraCloud。


### 准备工作

#### 【硬件准备】

按照25用户，人均8G内存，80G系统盘，推荐的服务器配置为：

- 处理器：E5-2680V4*2
- 内存：256G
- SSD：1.92TB * 2

上述规划中，虚拟机的系统盘放在 SSD。

#### 【软件准备】
- Proxmox 7.3 ISO文件    ： [中科大 Proxmox 7.3 下载地址](http://mirrors.ustc.edu.cn/proxmox/iso/)
- DoraCloud for Proxmox    ： [DoraCloud for PVE下载](https://www.doracloud.cn/downloads/2-cn.html)
- Windows平台客户端    ：[DoraClient客户端](https://www.doracloud.cn/downloads/doraclient-cn.html)

#### 【网络环境】 

- 局域网环境
- 网络中有DHCP，可以为桌面虚拟机分配IP
- 可以访问互联网


### 安装步骤

#### 第一步：安装DoraCloud管理系统

进入Proxmox的shell，执行如下命令可以完成在线安装。
```
cd /var/lib/vz/dump; wget -qO- https://dl.doracloud.cn/dpinstall.pl --referer https://doracloud.cn | perl
```
![](images/setup01.png?width=700px)

安装完毕后，系统会提示 DoraCloud 管理系统的IP地址。该地址为DHCP动态分配的IP。如下图：

![](images/setup02.png?width=700px)

如果网络中没有DHCP，那么需要手工修改IP地址，参考如下指导文档。

[https://docs.doracloud.cn/operation-maintenance/kb0001/](https://docs.doracloud.cn/operation-maintenance/kb0001/)

使用浏览器打开 DoraCloud 管理后台，输入账号  admin  密码 DoraCloud，登录DoraCloud管理系统。
![](images/setup03.png?width=700px)


#### 第二步：DoraCloud初始化配置

##### 1. 【欢迎页面】

登录后，显示欢迎页面。可以选择系统的语言，然后下一步。

![欢迎页面](images/setup04.png?width=700px)

![系统初始化](images/setup05.png?width=700px)

##### 2. 【配置虚拟化】

选择主机类型为Proxmox，输入Proxmox服务器的IP地址、用户名、密码。

![输入图片说明](images/setup06.png?width=700px)

##### 3. 【配置资源池】

选择存储池、网络池。 镜像存储池、数据盘存储池，默认值是与虚拟机存储池保持一致。

![输入图片说明](images/setup07.png?width=700px)

##### 4. 【配置集群】

创建一个新的DoraCloud集群

![输入图片说明](images/setup08.png?width=700px)

##### 5. 【配置用户数据库】

选择使用DoraCloud本地群组数据库

![输入图片说明](images/setup09.png?width=700px)

##### 6. 【为DoraCloud配置静态IP】

将DoraCloud管理系统的IP从DHCP修改成静态IP

![输入图片说明](images/setup10.png?width=700px)

网络设置界面如下，web界面可以在同一子网内修改IP地址。如果需要修改成其他网段的IP配置，就需要手工修改。

![输入图片说明](images/setup11.png?width=700px)

DoraCloud管理系统VM为 CentOS，请参考CentOS的静态IP设置方法。

也可以参见知识库KB0001的文档。

[https://docs.doracloud.cn/operation-maintenance/kb0001/](https://docs.doracloud.cn/operation-maintenance/kb0001/)


#### 第三步：下载模板，创建桌面池，用户

##### 1. 在线下载基础模板

进入【模板】主菜单，选择 win10ltsc2021v1 这个模板下载。

![模板仓库下载模板](images/setup12.png?width=700px)

下载成功后，模板菜单中可以看到 win10ltsc2021v1  模板信息

![模板回复成功](images/setup13.png?width=700px)

##### 2. 制作办公模板

从仓库下载的模板，不要直接使用。一个原因是仓库下载的模板只有基本的Windows系统，没有办公所需的应用软件。另一个原因是如果直接使用，后续仓库里的模板有更新时，就无法再下载到本地了。 因为本地已经有同名的模板存在了，不能进行覆盖。

推荐的做法是：对模板win10ltsc2021v1执行复制操作，创建一个名为win10office的模板。后续会在这个新的模板中安装办公所需的应用程序。

![复制模板](images/setup14.png?width=700px)

根据模板制作向导，进行模板的制作。

![编辑模板](images/setup15.png?width=700px)

按照提示信息，登录编辑中的模板虚拟机。在Windows开始菜单中，启动运行，输入 mstsc -v 192.168.1.204 

![输入图片说明](images/setup16.png?width=450px)

接下来登录模板虚拟机。 登录的账号为 administrator   123456 。这个账号是模板 win10ltsc2021v1 的缺省账号。 复制出来的新模板也继承了这个账号。

接下来的过程是在 Windows 模板中安装应用程序，比如Office，浏览器等。安装的过程，我们省去了截图。

应用程序安装完毕后，我们回到DoraCloud管理后台的模板制作向导，确认如下信息。

![输入图片说明](images/setup17.png?width=700px)

然后按下一步，选择模板类型为【专用和公用桌面】，接下来开始准备模板。

![输入图片说明](images/setup18.png?width=700px)

![输入图片说明](images/setup19.png?width=700px)

模板准备完毕后，可以测试模板，也可以选择保存模板，完成模板 win10office 的制作。

![输入图片说明](images/setup20.png?width=700px)

##### 3. 创建桌面池

根据项目规划，需要创建25个桌面，人均8G内存。接下来，我们选择 创建 桌面池。 内存设置为8G、处理器设置为1个处理器，4个核心。模板选择前一步创建好的 win10office 模板。 其他参数保持默认状态即可。

![输入图片说明](images/setup21.png?width=700px)

设置桌面池的创建策略。【最大创建】、【预创建】都设置成25。绑定账号设置成 administrator 123456 。 桌面池类型选择为【专用桌面】。

![输入图片说明](images/setup22.png?width=700px)

由于免费版不支持配置数据盘，在【配置存储】这个页面，直接选择下一步。 

![输入图片说明](images/setup23.png?width=700px)

在桌面池创建后，可以从【桌面计算机】菜单中观察桌面计算机状态。可以看到DoraCloud系统已经开始创建虚拟桌面。

![输入图片说明](images/setup24.png?width=700px)

##### 5. 创建群组和用户

首先创建一个群组group，这个群组签约了前面创建的桌面池。

![输入图片说明](images/setup25.png?width=700px)

然后创建一个测试用户 user01 。 user01 属于群组 group。

由于 group1 已经签约了桌面池，user01继承了group签约的桌面池。 不需要单独为user01 配置签约的桌面池。 点击提交，完成用户user01的创建。

![输入图片说明](images/setup26.png?width=700px)

#### 第四步：启用DoraCloud免费版，使用DoraClient连接桌面

##### 1. 启用免费版许可

DoraCloud安装后，缺省处于试用状态。如果你希望测试 DoraCloud 功能，可以在试用状态下测试。如果你希望长期免费试用，可以将软件许可切换成免费版。如下图操作。

![输入图片说明](images/setup27.png?width=700px)

下图是切换免费版之后的许可信息。切换成免费版本后，界面最上面的红色的试用信息会消失。

![输入图片说明](images/setup28.png?width=700px)

##### 2. 使用DoraClient连接桌面

DoraCloud桌面云可以通过多种方式的客户端登录。包括：
- 云终端/瘦客户机，如DC20、JC35、JC36等。
- 朵拉云DoraOS瘦客户机软件系统，用于将x86机器转换成基于Linux的瘦客户机。
- Windows平台的客户端 DoraClient。
- 直接浏览器访问，调用本地RDP客户端。

本方案采用的DoraClient。DoraClient是DoraCloud桌面云在Windows平台下的客户端软件。可以免费下载使用。

[DoraClient下载地址](https://www.doracloud.cn/downloads/doraclient-cn.html)

DoraClient 推荐在Windows 10以上版本使用，需要64位版本。不支持32位系统。

如果安装在 Windows 7 sp1 64位上，请先安装 kb3125574 。

安装完毕后运行 DoraClient，首先配置服务器地址。

![DoraClient配置服务器地址](images/setup29.png?width=700px)

然后设置输入账号，进行登录

![DoraClient登录](images/setup30.png?width=700px)

下图为DoraClient成功连接桌面。

![DoraCLient连接桌面](images/setup31.png?width=700px)

由于免费版本对登录的终端有限制，没有激活的DoraOS系统连接免费版的DoraCloud时，系统会自动注销。如果直接通过RDP客户端连接DoraCloud创建的桌面虚拟机，也会被自动注销。

如果使用 DoraClient 连接 DoraCloud 时，也遇到连接后自动注销的问题，请将 DoraClient 和 DoraCloud 都升级到 2022-12-15 后的版本。

除了使用 DoraClient登录，也可以购买朵拉云的云终端作为客户的端。或者购买DoraOS瘦客户机系统，将x86主机改造成云终端。

### 方案总结

DoraCloud免费版搭配DoraClient客户端可以构成完全免费的解决方案。 适合如下一些应用场景：

- 现有的PC，旧PC，安装DoraClient作为客户端，接入桌面云。 
- 购买廉价的x86盒子，安装Windows和DoraClient作为瘦客户机，接入桌面云。
- 员工出差，或者居家办公，使用笔记本或者家用电脑安装DoraClient远程接入。
- 简单的桌面云应用场景，用户有一定的动手能力。 

DoraCloud免费版是一款超值的桌面云方案。如果需要如下特性，可以考虑付费的商业版本。

1. 需要原厂的方案设计、部署和技术支持服务。
2. 需要支持用户数据盘。 
3. 需要支持USB重定向。
4. 并发用户超过25。
5. 需要配置桌面网关，实现网络隔离。
6. 需要支持3D应用，需要配置vGPU。

