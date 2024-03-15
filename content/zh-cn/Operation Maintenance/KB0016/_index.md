
---
title: "KB0016. DoraCloud管理系统没有IP地址，内部只有eth1网卡"
linkTitle: "KB0016. DoraCloud管理系统没有IP地址，内部只有eth1网卡"
date: 2022-11-28
weight: 16
description: >
   DoraCloud管理系统没有IP地址，内部只有eth1网卡
---


DoraCloud 管理系统的Linux VM的mac地址如果发生改变。会导致会导致虚拟机内的网卡变成了eth1，DoraCloud管理系统无法正常启动。
出现这个问题的原因时，CentOS Linux会将mac地址和eth0、eth1绑定。这样老的mac绑定了eth0，新的mac绑定eth1，从而无法得到正确的ip配置。

问题的解决方案为，进入 DoraCloud 管理系统的Linux 。缺省账号 root  dora@cloud
然后执行一下操作

```
rm vi /etc/udev/rules.d/70-persistent-net.rules
reboot
```


重启 Linux 后，观察IP是否正常。 

