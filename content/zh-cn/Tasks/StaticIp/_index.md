---
title: "Deskpool静态ip组网"
linkTitle: "Deskpool静态ip组网"
date: 2020-04-09
type: "docs"
weight: 6
description: >
   本文描述了使用静态ip配置部署桌面云的过程和注意事项。
---

### 1.前言   
Deskpool桌面云系统一般使用DHCP机制来分配桌面的IP地址。在一些项目中，不允许使用DHCP，采用静态IP的管理方式。本文描述了Deskpool使用静
态IP组网时的注意事项。 
本文假定您已经了解了Deskpool的安装和使用。本文不详细描述Deskpool的完整配置过程，仅描述使用静态IP组网的配置要点。
本文以配置一个50用户的云教室为例，描述了使用静态IP组网的配置过程。


### 2.网络规划
在使用静态IP配置前，需要进行网络IP地址的规划。

一个50用户云教室中有如下主机需要配置IP地址：
物理服务器、Deskpool管理VM、50个桌面、50个终端、至少一个模板。
上述物理或者虚拟主机都需要分配独立的IP。
我们假定网段为 192.168.1.x/24，网关和DNS为 192.168.1.1。我们对该桌面云系统的IP规划如下

1. 对局域网进行网段规划，以192.168.1.1/24为例，我们需要对哪些网络分别给到不同的IP地址，例如我们把192.168.1.20这个网络给物理服务器作为ip地址，192.168.1.21给到deskpool云桌面系统，192.168.1.150--192.168.1.200的网络给到云桌面，192.168.1.201—192.168.1.250的网络
给到云终端。

| 网段         | 192.168.1.X                  |
| ------------ | ---------------------------- |
| 掩码         | 255.255.255.0                |
| 网关         | 192.168.1.1                  |
| DNS          | 192.168.1.1                  |
| 物理服务器IP | 192.168.1.20                 |
| Deskpool IP  | 192.168.1.21                 |
| 虚拟桌面IP   | 192.168.1.150--192.168.1.200 |
| 云终端IP     | 192.168.1.201--192.168.1.250 |
| 教师机IP     | 192.168.1.24                 |
| 云桌面模板IP | 192.168.1.22                 |


### 3.设置物理服务器IP

3.1.    物理服务器安装系统后，我们首先要进行设置固定ip，如图下

{{< figure src="./images/doracloud_1.png" width="600">}}

{{< figure src="./images/doracloud_2.png" width="600">}}

{{< figure src="./images/doracloud_3.png" width="600">}}

{{< figure src="./images/doracloud_4.png" width="600">}}


### 4.设置deskpool虚拟化系统ip

4.1	在安装deskpool中会弹出deskpool设置窗口，我们根据网络规划表来设置。如图下 

{{< figure src="./images/doracloud_5.png" width="600">}}

4.2	进入deskpool虚拟管理系统将ip改成规划ip。如图下  

{{< figure src="./images/doracloud_6.png" width="600">}}

4.3     设置好ip后，再次登陆（输入修改后的ip地址）。注意，我们接下来恢复模板的时候也需要deskpool的ip。如图下

{{< figure src="./images/doracloud_7.png" width="600">}}


### 5.设置模板IP

5.1     首先在模板的菜单栏里剪辑模板的编辑，由于没有设置静态IP，是无法正常进入；我们需要打开hyper-V管理器，找到该模板的虚拟机，连接进入虚拟机设置成静态IP，如图下

{{< figure src="./images/doracloud_8.png" width="600">}}

{{< figure src="./images/doracloud_9.png" width="600">}}

{{< figure src="./images/doracloud_10.png" width="600">}}


### 6.设置桌面池IP

6.1     在桌面池菜单栏里，点击属性进入桌面池设置，将IP绑定成固定IP。如图下

{{< figure src="./images/doracloud_11.png" width="600">}}

### 7.设置云终端IP

7.1     最后，我们将云终端上的IP设置为固定IP。如图下


{{< figure src="./images/doracloud_12.png" width="600">}}



### 8.常见问题

8.1     模板一定要设置好固定IP，不然在保存模板时会出现错误导指保存不了。

8.2     在云教室搭建布置时，一定要先将网络规划表做一个规划。
