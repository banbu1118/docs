---
title: "ShareStation工作站虚拟化部署"
linkTitle: "ShareStation工作站虚拟化部署"
date: 2019-12-20
weight: 6
aliases:
    - /docs/installation-upgrade/sharestation/
description: >
   ShareStation是朵拉云推出的工作站虚拟化方案。 本文描述了利用DoraCloud桌面虚拟化系统和Proxmox虚拟化平台部署ShareStation工作站虚拟化方案的过程。
---
ShareStation®是朵拉云科技针对设计行业推出的高性价比3D桌面方案。ShareStation可以让工作站虚拟化后分配给多个用户使用。ShareStation®方案包括设计工作站、高性能云终端和DoraCloud桌面云软件。ShareStation可以为企业的设计人员提供虚拟的工作站，确保企业信息安全，并且支持远程设计和办公。

Proxmox virtualization environment，简称PVE，是一个开源免费的基于linux的企业级虚拟化方案，功能不输专业收费的VMware。简单的说，PVE是一个基于Debian的linux系统，内置了一套虚拟机管理工具，并提供了web管理页面，让我们可以非常简单的通过网页管理虚拟机。

DoraCloud是一套多平台、一体化、分布式的桌面虚拟化方案。基于开放架构，支持多种虚拟化平台（Hyper-V，VMware，Proxmox，XenServer），多种桌面协议（RDP，PCoIP，SPICE）。采用All-in-One的设计模式和虚拟设备的部署方式。

NVIDIA Tesla P4单精度运算能力将达到5.5FLOPS，每秒可进行22万亿次计算，其拥有2560个流处理器，搭配8GB GDDR5显存。


准备条件
1、一台Dell T3640 工作站，配置 i7-10700，64G， 1TB SSD， Tesla P4卡。
2、一个启动U盘。
3、网络内有DHCP服务。

本文描述在ShareStation工作站虚拟化方案的部署过程。 将服务器上部署 Proxmox、DoraCloud，并创建带有vGPU的虚拟桌面。


### 1、下载安装Proxmox 7.4.1

 
推荐中科大（ USTC）的源下载 ISO。 

`https://mirrors.ustc.edu.cn/proxmox/iso/`

U盘启动制作工具推荐 Rufus 或者 Ventoy。entoy作为新一代U盘启动工具，不需要反复对U盘进行格式化。只需要把ISO拷贝到Ventoy制作好的U盘上即可。

工作站开机，按F12，选择U盘启动，进入Ventoy启动菜单。选择Proxmox VE 7.4.1 的ISO镜像启动。进入Proxmox的安装过程。


### 2、安装显卡驱动，并部署DoraCloud桌面管理系统
1）修改Proxmox的安装源，并执行更新。安装 pve-headers、dkms等包。

`curl -o- http://www1.deskpool.com:9000/software/gpu01.sh |bash`
 
2）启动IO-MMU

`curl -o- http://www1.deskpool.com:9000/software/gpu02.sh |bash`

执行脚本后，会自动重启服务器。
 
3）安装nvidia vGPU显卡驱动。

NVIDIA vGPU的驱动有两个选择，以下脚本安装GRID 16.2(-535.129.03)的驱动

`curl -o- http://www1.deskpool.com:9000/software/gpu03f.sh |bash`

以下脚本安装 GRID 13.6 ( 470.161.02)的驱动。

`curl -o- http://www1.deskpool.com:9000/software/gpu03e.sh |bash`

执行脚本后，会自动重启服务器。
 
4）安装DoraCloud 管理系统

可以使用官网的在线安装脚本安装

`cd /var/lib/vz/dump; wget -qO- https://dl.doracloud.cn/dpinstall.pl --referer https://doracloud.cn | perl`

重启完毕后，然后浏览器登录 DoraCloud 管理后台，输入 账号  admin  DoraCloud，登录后台。

{{< figure src="./images/sharestation-1.png"  width="600" >}} 
 

根据配置向导，完成DoraCloud的初始化配置。

{{< figure src="./images/sharestation-2.png"  width="600" >}} 
{{< figure src="./images/sharestation-3.png"  width="600" >}} 
{{< figure src="./images/sharestation-4.png"  width="600" >}} 
{{< figure src="./images/sharestation-5.png"  width="600" >}} 
{{< figure src="./images/sharestation-6.png"  width="600" >}} 

 

接下来，我们下载支持vGPU的桌面模板。 win10LTSC2019GPU。
{{< figure src="./images/sharestation-7.png"  width="600" >}} 


 

  然后创建桌面池，选择 win10LTSC2019GPU这个模板。 GPU型号选择 NVIDIA P4，vGPU类型选择 GRID P4-1Q

{{< figure src="./images/sharestation-8.png"  width="600" >}} 

 

 

 配置桌面池内创建4个桌面。然后设置桌面池的绑定账号为 administrator  123456 。这样账号是windows7x64模板的Windows 账号。

启用绑定账号后，终端可以识别这个绑定账号，登陆桌面windows。
{{< figure src="./images/sharestation-9.png"  width="600" >}} 
 

 桌面创建完毕后，可以在PVE中查看桌面虚拟的硬件配置，确认桌面虚拟机正常配置了 PCI device。

 
{{< figure src="./images/sharestation-10.png"  width="600" >}} 
  

 接下来回到DoraCloud管理后台，添加用户，为用户分配桌面池。

 {{< figure src="./images/sharestation-11.png"  width="600" >}} 

 

### 5、登录桌面，验证vGPU效果
DoraCloud有多种登录方式，我们选择网页登录DoraCloud，输入用户账号  user01，密码123456。 然后打开一个 RDP 文件，输入管理员账号 administrator  123456，即可登录 windows 桌面。

{{< figure src="./images/sharestation-12.png"  width="600" >}} 

 

 进入桌面后，通过dxdiag，查看系统的显卡，显示为 NVIDIA GRID P4-1Q。


{{< figure src="./images/sharestation-13.png"  width="600" >}} 
 

接下来，可以进行3D性能的测试了。 推荐两个在线测试的网站。

基于WebGL的水母   　　　　https://akirodic.com/p/jellyfish/

基于WebGL的网页游戏　　　　https://www.crazygames.com/

{{< figure src="./images/sharestation-14.png"  width="600" >}} 
{{< figure src="./images/sharestation-15.png"  width="600" >}} 

 



 
