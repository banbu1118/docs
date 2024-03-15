---
title: "KB0019. 使用ceph作为数据盘存储池时，用户数据盘无法创建"
linkTitle: "KB0019. 使用ceph作为数据盘存储池时，用户数据盘无法创建"
date: 2022-12-12
weight: 19
description: >
    使用ceph作为数据盘存储池时，用户数据盘无法创建"
---

 **Proxmox VE 最新版本 7.4.3 已经修复了改bug。可以通过 apt dist-upgrade 升级proxmox 到最新版本解决** 


Proxmox 的API在处理 ceph 存储时，有bug。需要要修改一下proxmox 中的脚本。执行如下命令

```
cp /usr/share/perl5/PVE/Storage/RBDPlugin.pm /usr/share/perl5/PVE/Storage/RBDPlugin.pm.org 

sed -i  "s/return \$size;/return wantarray ? (\$size, 'raw', 0, undef) : \$size;/g"  /usr/share/perl5/PVE/Storage/RBDPlugin.pm

```

上述命令会完成如下操作：

将文件/usr/share/perl5/PVE/Storage/RBDPlugin.pm 的volume_size_info 函数从如下内容

```
sub volume_sizeinfo {
    my ($class, $scfg, $storeid, $volname, $timeout) = @;

    my ($vtype, $name, $vmid) = $class->parse_volname($volname);
    my ($size, undef) = rbd_volume_info($scfg, $storeid, $name);
    return $size;
}

```

修改成


```
sub volume_sizeinfo {
    my ($class, $scfg, $storeid, $volname, $timeout) = @;

    my ($vtype, $name, $vmid) = $class->parse_volname($volname);
    my ($size, undef) = rbd_volume_info($scfg, $storeid, $name);
    return wantarray ? ($size, 'raw', 0, undef) : $size;
}

```
