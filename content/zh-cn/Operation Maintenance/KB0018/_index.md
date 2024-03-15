
---
title: "KB0018. 如何修改DoraCloud桌面云的RDP服务端口"
linkTitle: "KB0018. 如何修改DoraCloud桌面云的RDP服务端口"
date: 2019-12-20
weight: 18
description: >
   如何修改DoraCloud桌面云的RDP服务端口
---

在一些应用场景中，不允许开放3389端口。这种情况下，我们可以修改RDP服务的端口。



### 1、启用BIOS中，处理器设置的 Virtualization Technology 和 IOMMU Support

![](./images/dora_vgpu02.png?width=700px)

### 2、启用BIOS中，集成外设（Integrate Devices)的 SR-IOV Global Enable。

![](./images/dora_vgpu01.png?width=700px)


