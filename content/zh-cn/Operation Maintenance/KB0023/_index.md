---
title: "KB0023.pve直接编辑磁盘内容的两种方式"
linkTitle: "KB0023.pve直接编辑磁盘内容的两种方式"
date: 2024-1-6
weight: 23
description: >
  直接编辑磁盘内容的两种方式
---



### 方法一：libguestfs

1.  安装libguestfs工具集

```
#安装libguestfs工具集
apt install libguestfs-tools
```

2.编辑虚拟机磁盘文件

```
#虚拟机关机状态操作
#如果是windows虚拟机，如果开机过一次，就需要关闭电源管理里的快速启动选项（快速启动会把内存数据写入磁盘保存，导致虚拟机磁盘无法编辑）

#创建挂载点
mkdir /kk

#挂载磁盘
guestmount  -a /mnt/data/images/103/vm-103-disk-1.qcow2 -i --rw /kk
```

1.  编辑模板磁盘文件

```
#解除模板磁盘文件锁定
chattr -i base-114-disk-1.qcow2

#创建挂载点
mkdir /kk

#挂载磁盘
guestmount -a base-114-disk-1.qcow2 -i --rw /kk
```

1.  编辑磁盘内容

```
#进入挂载点
cd /kk

#编辑磁盘内容
```

1.  移除挂载



    umount /kk

### 方法二：qemu-nbd

1.  安装qemu-utils（pve自带，不用安装）,安装nfts-3g

```
#安装qemu-utils
apt install qemu-utils

#安装ntfs-3g
apt install ntfs-3g
```

2.创建nbd网络块设备

    #加载nbd内核模块，并创建8个nbd块设备
    modprobe nbd max_part=8

3.将磁盘导出为网络块设备

```
#将磁盘导出为网络块设备
qemu-nbd --connect=/dev/nbd0 /mnt/data/images/102/vm-102-disk-1.qcow2
```

1.  查看nbd块分区情况

```
#查看nbd块分区情况
fdisk -l /dev/nbd0

#windows分区一般有3个
root@pve:/kk# fdisk -l /dev/nbd0
Disk /dev/nbd0: 100 GiB, 107374182400 bytes, 209715200 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: EA0D45E0-A220-4547-8D89-7B1E34142D39

Device       Start      End  Sectors  Size Type
/dev/nbd0p1   2048   206847   204800  100M EFI System
/dev/nbd0p2 206848   239615    32768   16M Microsoft reserved
/dev/nbd0p3 239616 62910494 62670879 29.9G Microsoft basic data
```

1.  挂载虚拟机磁盘和模板磁盘文件

```
##虚拟机关机状态操作
#如果是windows虚拟机，如果开机过一次，就需要关闭电源管理里的快速启动选项（快速启动会把内存数据写入磁盘保存，导致虚拟机磁盘无法编辑

#挂载虚拟机磁盘，nbd00p3为windows系统分区
mount -t ntfs /dev/nbd0p3 /kk

#挂载模板磁盘（同理）
#解除模板磁盘文件锁定
chattr -i /mnt/data/images/114/base-114-disk-1.qcow2 
qemu-nbd --connect=/dev/nbd0 /mnt/data/images/114/base-114-disk-1.qcow2 
mount -t ntfs /dev/nbd0p3 /kk
```

1.  编辑磁盘内容

```
#进入挂载点
cd /kk

#编辑磁盘内容
```

7.移除挂载

```
#移除/kk挂载点
umount /kk

#移除块设备挂载
qemu-nbd --disconnect /dev/nbd0
```

