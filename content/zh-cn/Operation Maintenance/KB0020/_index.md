---
title: "KB0020. 使用ZFS作为数据盘存储池时，用户数据盘无法创建"
linkTitle: "KB0020. 使用ZFS作为数据盘存储池时，用户数据盘无法创建"
date: 2022-12-12
weight: 20
description: >
    使用ZFS作为数据盘存储池时，用户数据盘无法创建"
---

 **最新的Proxmox VE版本（7.4.3)已经修复了这个bug。 执行   apt dist-upgrade 即可。   2023-06-04 ** 


Proxmox 的API在处理 ZFS 存储时，有bug。 

首先执行如下命令，备份需要修改的文件 ZFSPoolPlugin.pm


```
cp /usr/share/perl5/PVE/Storage/ZFSPoolPlugin.pm /usr/share/perl5/PVE/Storage/ZFSPoolPlugin.pm.origin

```

找到/usr/share/perl5/PVE/Storage/ZFSPoolPlugin.pm 的 volume_size_info 函数，内容如下

```
sub volume_size_info {
    my ($class, $scfg, $storeid, $volname, $timeout) = @_;

    my (undef, $vname, undef, undef, undef, undef, $format) =
        $class->parse_volname($volname);

    my $attr = $format eq 'subvol' ? 'refquota' : 'volsize';
    my $value = $class->zfs_get_properties($scfg, $attr, "$scfg->{pool}/$vname");
    if ($value =~ /^(\d+)$/) {
	return $1;
    }

    die "Could not get zfs volume size\n";
}
```

修改成如下内容


```

sub volume_size_info {
    my ($class, $scfg, $storeid, $volname, $timeout) = @_;

    my (undef, $vname, undef, undef, undef, undef, $format) =
        $class->parse_volname($volname);

    my $size = 0;
    my $used = 0;
    my $attr = $format eq 'subvol' ? 'refquota' : 'volsize,used,origin,name';
    my $value = $class->zfs_get_properties($scfg, $attr, "$scfg->{pool}/$vname");
    if ($value =~ /^(\d+)$/) {
        $size = $1;
    }

    if ($value =~ /^(\d+)$/) {
        $used = $1;
    }
    
    return wantarray ? ($size,$format,$used,0,undef) : $size;
}

```






