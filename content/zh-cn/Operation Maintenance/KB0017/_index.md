
---
title: "KB0017. 戴尔服务器R7525启用支持NVIDIA GPU，开启vGPU"
linkTitle: "KB0017. 戴尔服务器R7525启用支持NVIDIA GPU，开启vGPU"
date: 2019-12-20
weight: 17
description: >
   戴尔服务器R7525启用支持NVIDIA GPU，开启vGPU
---

戴尔服务器R7525启用NVIDIA GPU，服务器的BIOS需要做如下设置：

### 1、启用BIOS中，处理器设置的 Virtualization Technology 和 IOMMU Support

![](./images/dora_vgpu02.png?width=700px)

### 2、启用BIOS中，集成外设（Integrate Devices)的 SR-IOV Global Enable。

![](./images/dora_vgpu01.png?width=700px)


