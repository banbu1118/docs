
---
title: "KB0002.对DoraCloud的桌面模板系统盘进行扩容"
linkTitle: "KB0002.对DoraCloud的桌面模板系统盘进行扩容"
date: 2019-12-20
weight: 2
description: >
   对DoraCloud的桌面模板系统盘进行扩容
---


DoraCloud的桌面是根据桌面模板创建的。桌面模板包含了系统盘。有时系统盘的空间不满足需求，就需要对系统盘大小进行调整。

我们以Hyper-V平台的DoraCloud为例，扩充模板 win7x86base 这个模板的空间。

首先，在DoraCloud管理后台中编辑模板，进入模板编辑状态。
{{< figure src="./images/doracloud_expand_disk1.png" width="600">}}

然后，进入服务器的Hyper-V管理系统，找到正在编辑中的模板，虚拟机名称为 win7x86base，执行【关机】操作，然后点击【设置】。

{{< figure src="./images/doracloud_expand_disk2.png" width="600">}}

选择磁盘驱动器，【编辑】。


{{< figure src="./images/doracloud_expand_disk3.png" width="600">}}

在磁盘编辑菜单中，选择【扩展】，将磁盘容量扩展到 80G。

{{< figure src="./images/doracloud_expand_disk4.png" width="600">}}

{{< figure src="./images/doracloud_expand_disk5.png" width="600">}}

之后保存配置，启动 win7x86base ，回到DoraCloud管理界面，mstsc连接正在运行的桌面模板，进入windows。

{{< figure src="./images/doracloud_expand_disk6.png" width="600">}}

选择磁盘0，【扩展卷】操作。

{{< figure src="./images/doracloud_expand_disk7.png" width="600">}}

DoraCloud在线模板库提供的模板，一般在虚拟硬件上配置了50-60GB的系统盘空间。 但是在创建桌面操作系统时，实际分配了大约20-30GB，还有一些预留空间。

这种情况下，可以直接编辑模板，才模板内执行【扩展卷】操作。
