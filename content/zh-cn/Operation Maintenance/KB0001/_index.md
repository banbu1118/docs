
---
title: "KB0001.修改DoraCloud管理系统的IP地址"
linkTitle: "KB0001.修改DoraCloud管理系统的IP地址"
date: 2019-12-20
weight: 1
description: >
   修改DoraCloud管理系统的IP地址
---

DoraCloud 管理系统是一个CentOS Linux的虚拟机。我们既可以通过DoraCloud后台管理系统修改它的IP地址，也可以通过CentOS命令修改它的IP地址。


#### 方法1：在DoraCloud的管理系统中修改IP地址
在DoraCloud后台，依次选择【服务器】【属性】【DoraCloud网络】【静态IP配置】，即可修改网络IP地址。

这种方法修改IP地址有限制。仅仅可以修改成同一子网内的IP地址。

{{< figure src="./images/doracloud_change_ipaddr.png" width="600">}}

#### 方法2：在DoraCloud Linux VM中通过nmtui程序修改
进入DoraCloud VM的虚拟机控制台。
登录DoraCloud VM。账号为 root   dora@cloud
输入命令 nmtui，即可启动一个程序，修改IP地址。

{{< figure src="./images/doracloud_change_ipaddr_nmtui1.png" width="600">}}

然后在nmtui中修改，如下图。


{{< figure src="./images/doracloud_change_ipaddr_nmtui2.png" width="600">}}

修改完毕后，选择 save，quit，即可。



#### 方法3：在DoraCloud Linux VM中通过配置文件修改
该方法需要您掌握基本的Linux命令，如vi的用法。 您可以参考有关 CentOS 7修改IP地址的资料。

进入DoraCloud VM的虚拟机控制台。 登录DoraCloud VM。账号为 root   dora@cloud
进入网络配置文件network-scripts目录下。

```
[root@vdimgr ~]# vi /etc/sysconfig/network-scripts/ifcfg-eth0
```


{{< figure src="./images/doracloud_change_ipaddr_vi1.png" width="600">}}

通过 vi命令，打开 网卡的配置文件


{{< figure src="./images/doracloud_change_ipaddr_vi2.png" width="600">}}

修改完毕后，输入 :wq!  ，即可保存并退出。 然后输入 reboot 重启 DoraCLoud Linux VM 主机。 
