
---
title: "KB0004.如何进行DoraCloud版本升级？"
linkTitle: "KB0004.如何进行DoraCloud版本升级？"
date: 2019-12-20
weight: 4
description: >
   KB0004.如何进行DoraCloud版本升级？
---

升级过程为：
##### 1）.现有版本，进入维护模式，导出系统数据。 

![](./images/doracloud_upgrade1.png) 
![](./images/doracloud_upgrade2.png) 
![](./images/doracloud_upgrade3.png) 

##### 2）.记录现当前版本DoraCloud VM 的IP地址，子网掩码、网关、DNS信息，将VM关机。

![](./images/doracloud_upgrade4.png) 

##### 3）.安装新版本DoraCloud，在向导的第一步，选择“系统迁移”，选择之前导出的数据导入。

![](./images/doracloud_upgrade5.png) 

##### 4）.根据向导提示，完成向导。

![](./images/doracloud_upgrade6.png) 
![](./images/doracloud_upgrade7.png) 
![](./images/doracloud_upgrade8.png) 

##### 5）.然后设置 DoraCloud VM的IP地址为固定IP。

![](./images/doracloud_upgrade10.png) 

##### 6）.退出维护模式。

![](./images/doracloud_upgrade9.png) 


