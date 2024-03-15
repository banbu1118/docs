
---
title: "KB0006.如何通过云终端的共享打印机"
linkTitle: "KB0006.如何通过云终端的共享打印机"
date: 2019-12-20
weight: 6
description: >
   如何通过云终端的共享打印机
---

DoraCloud云终端内置打印服务器程序，可以通过高层重定向进行共享打印。

安装的大致步骤如下：

 **第一步、首先把云终端设置成固定IP地址，并把打印机通过USB接口与云终端进行连接。打印机通电。** 

本文操作的云终端的固定IP地址配置成了192.168.1.151。  云终端打印服务的web管理地址为 http://192.168.1.151:631 。

 **第二步、然后登陆云桌面，在windows内通过浏览器配置云终端的打印机。把打印机类型选择为 RAW，并且勾选允许共享。 记录打印机的 URL 地址。** 

浏览器访问云终端打印机服务的后台 http://192.168.1.151:631， 选择添加打印机
{{< figure src="./images/doraos_print1.png" width="600">}}

选中云终端识别的打印机，类型为HP LaserJet Professional P1108
{{< figure src="./images/doraos_print2.png" width="600">}}

勾选 Share This Printer，然后继续
{{< figure src="./images/doraos_print3.png" width="600">}}

接下来注意，制造商选择 RAW 。表示透传打印机。然后添加打印机。
{{< figure src="./images/doraos_print4.png" width="600">}}
{{< figure src="./images/doraos_print5.png" width="600">}}

点击进入添加成功的打印机  HP_LaserJet_Professional_P1108 ，进入打印机页面。
{{< figure src="./images/doraos_print6.png" width="600">}}

最后我们这个打印机的页面地址（URL)复制下来。用于后续打印机的添加。
{{< figure src="./images/doraos_print7.png" width="600">}}


 **第三步、在Windows内安装打印机驱动。安装驱动后，打印安装程序会提示检测打印机的USB，这时可以退出。 启动已经安装成功。** 
{{< figure src="./images/doraos_print8.png" width="600">}}
{{< figure src="./images/doraos_print9.png" width="600">}}

 **第四步、通过Windows的添加网络打印机功能，添加打印机的URL。更具打印机的型号，选择正确的打印机的厂家和类型。即可完成打印驱动完成。** 

在控制面板中，添加打印机，选择网络打印机，然后选择“我需要的打印机不在列表中”。
{{< figure src="./images/doraos_print10.png" width="600">}}

粘贴第二步获得的打印机的URL。
{{< figure src="./images/doraos_print11.png" width="600">}}

选择正确的打印机厂家和型号，然后打印机就添加完毕。
{{< figure src="./images/doraos_print12.png" width="600">}}

可以 测试一下打印。以确认打印机安装成功。
{{< figure src="./images/doraos_print13.png" width="600">}}

