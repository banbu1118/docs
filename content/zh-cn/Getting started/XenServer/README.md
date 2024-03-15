Deskpool 是一款桌面虚拟化管理软件，支持Hyper-V、XenServer等虚拟化平台。Deskpool采用一体化设计理念，把桌面虚拟化所需的组件打包在一个虚拟机镜像中，极大的简化了虚拟桌面部署的难度。

Deskpool 3.1 for Hyper-V 提供vdibackup工具，可以用于Deskpool的快速部署。本文描述了下载Deskpool备份档案，并快速部署 Deskpool桌面云环境的过程。

  **安装前准备** 

 1.一台服务器，配置为 :CPU i5 以上，内存 ≥16GB，硬盘为SSD或者RAID。在BIOS中开启CPU的虚拟化支持“Intel VirtualTechnology”

 2.网络中开启了 DHCP 服务。

 3.Windows Server 2012 R2 ISO镜像 官方下载地址： http://technet.microsoft.com/zh-cn/evalcenter/dn205286.aspx 另外msdn.itellyou.cn网站也提供了P2P下载Windows Server安装包的地址。 Windows Server 2012R2的 ISO文件：cn_windows_server_2012_r2_vl_x64_dvd_3316795.iso ed2k://|file|cn_windows_server_2012_r2_x64_dvd_2707961.iso|4413020160|010CD94AD1F2951567646C99580DD595|/

 4.下载并安装 Deskpool 3.2。

Deskpool 3.2 for Hyper-V 的下载地址为：Deskpool下载地址

 5.下载 Demo 环境的备份档案，并使用 Demo 镜像恢复桌面云环境。

Demo备份档（3.1GB）下载地址

Backup001备份档案：地址一

Backup001备份档案：地址二

 **U盘制作：** 

 1）使用UltraISO制作Windows Server启动U盘。

 2）使用Windows 7 USB DVD Download Tool制作Windows Server启动U盘。

 **  步骤一：安装Windows Server 2012R2 和原厂网卡、主板驱动程序** 

安装 Windows Server 2012R2的过程比较简单，注意几点：

1.选择标准版（带有GUI的服务器）或者 数据中心版本（带有GUI的服务器）。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/185715_aa230fad_6517935.png "hyperv_2012r2_setup1.png")

2.选择自定义安装，如果有分区，则删除已有的分区。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/185817_96e1351a_6517935.png "屏幕截图.png")

3.安装完成后，需要设置管理员账户的密码。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/185851_f9a0f301_6517935.png "屏幕截图.png")

   **步骤二：安装 Hyper-V 角色，创建虚拟交换机，确保网络中有DHCP服务** 

Hyper-V角色可以手工通过Windows的“服务器管理”安装，也可以通过Deskpool向导自动安装Hyper-V。

通过“服务器管理”安装的过程如下：
 
 1.打开“服务器管理”，管理页面如下图所示 点击勾选的区域“添加角色和功能”

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190331_72312d1c_6517935.png "屏幕截图.png")

 2.进入安装步骤

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190428_4a4634b0_6517935.png "屏幕截图.png")

 3.选择“基于角色或基于功能的安装”

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190449_c7534911_6517935.png "屏幕截图.png")

 4.选择“Hyper-V”

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190531_63e28cee_6517935.png "屏幕截图.png")

 5.勾选选择“以太网”

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190552_ad885c5e_6517935.png "屏幕截图.png")

向导执行完毕后，会提示需要重启服务器，服务器重启后，可以完成 Hyper-V角色的安装。

  ** 步骤三：下载并安装 Deskpool 桌面虚拟化管理系统** 

Deskpool 3.2 for Hyper-V 的下载地址为：Deskpool下载地址

 1.执行安装向导，选择“下一步”，会启动Deskpool安装程序。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190715_7427a47a_6517935.png "屏幕截图.png")

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190732_df64c288_6517935.png "屏幕截图.png")

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190745_9966224a_6517935.png "屏幕截图.png")
 
 2.选择存储池和虚拟网络。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190851_0b8c071b_6517935.png "屏幕截图.png")

 3.完成安装

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/190914_40b6687b_6517935.png "屏幕截图.png")

 **  步骤四：下载 Demo 环境的备份档案，并使用 Demo 镜像恢复桌面云环境** 

Demo备份档（3.1GB）下载地址

Backup001备份档案：地址一https://yangtzi.vicp.net:843/downloads/software/backup001.zip

Backup001备份档案：地址二http://deskpool.vicp.io:9000/software/backup001.zip

Demo备份档包括一个 windows 7 x86 Ultimate sp1系统的镜像，配置了一个办公桌面池，一个教学桌面池。

恢复过程为：

 1.解压 backup001.zip 到磁盘，比如 C:\。

进入backup001目录，可以看到数据库备份文件 deskpool.tar.gz，以及桌面模板的备份目录 win7x86base

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191306_1a8811fb_6517935.png "屏幕截图.png")

在地址栏输入 vdibackup，启动vdibackup

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191324_13e49858_6517935.png "屏幕截图.png")

 2.选择恢复数据库。

点击左侧“恢复”按钮，进入恢复界面。然后输入“桌面管理虚拟机IP”，然后点击“刷新”，观察底部提示，如果与VDI服务器建立了联系，并从VDI取得了存储池配置，属于正常状态。
!
[输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191347_d6973dbb_6517935.png "屏幕截图.png")

 3.勾选需要恢复的桌面模板，可以选择全选。

在右边的存储池配置中选择正确的存储池路径。通常情况下，虚拟机存储池放和镜像存储池在 SSD 磁盘上。数据盘存储池可以放在HDD磁盘上。虚拟机存储池为必须填写的选项。数据盘和镜像的存储池如果不填写，等于虚拟机存储池。
!
[输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191420_918c8bd3_6517935.png "屏幕截图.png")

 4.点击下方的“恢复”按钮开始恢复。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191451_7e093796_6517935.png "屏幕截图.png")

注意：恢复成功后，Hyper-V管理系统中会产生一个虚拟机，名字为“win7x86basev185f639”的虚拟机。 该虚拟机是模板，请千万不要启动该虚拟机!

 **  步骤五：用浏览器访问Deskpool虚拟桌面管理系统的管理后台（Admin Portal)** 

注意，是填写Deskpool管理系统的IP地址，不是Windows Server的IP地址。缺省账号为：用户 admin，密码 deskpool。 在Deskpool系统查看“桌面计算机”，可以看到系统正在创建虚拟桌面，等待创建完毕。 

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191537_3cd20779_6517935.png "屏幕截图.png")

   **步骤六：通过朵拉云瘦客户机或者DoraOS瘦客户机软件系统连接Deskpool桌面云** 

这里以朵拉云Deskpool专用终端DC20为例，连接 Deskpool 系统。如果没有朵拉云DC20云终端，可以在一个x86机器上部署DoraOS系统，把机器改造成瘦客户机。DoraOS的下载和部署参考如下链接 DoraOS下载和安装。

 1.连接朵拉云瘦客户机

 2.打开朵拉云瘦客户机，进入设置页面
!
[输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191622_a8e62bd2_6517935.png "屏幕截图.png")

 3.点击“Deskpool”图标，设置服务器地址和连接名称。
!
[输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191645_be6b892b_6517935.png "屏幕截图.png")

 4.设置完成，点击红框区域的“连接”，进入Deskpool系统。
!
[输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191705_90f64f70_6517935.png "屏幕截图.png")

 5.然后以 用户名user01 密码123456 登陆，即可连接桌面。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191720_9db2b666_6517935.png "屏幕截图.png")

 6.选择桌面，点击即可进入系统。

![输入图片说明](https://images.gitee.com/uploads/images/2020/0304/191734_69d77a6e_6517935.png "屏幕截图.png")

此外，您也可以通过浏览器直接登陆Deskpool桌面云。直接在登陆页面输入用户账号，点击桌面图标，会自动下载RDP文件。打开RDP文件，输入桌面账号： 用户 administrator 密码 123456，即可进入桌面系统。
