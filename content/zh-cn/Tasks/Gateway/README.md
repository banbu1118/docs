 
DoraCloud for windows20012R2远程桌面网关配置手册
 
 
一、简介 
终端服务网关（TS 网关）是一个终端服务子角色，允许经授权的远程用户通过 Internet 连接到企业网络中的终端服务器和工作站，RDP连接程序基于HTTPS实现客户端与终端服务器之间的通讯。这样，组织可轻松又安全地使远程用户或外出工作 的人员无需使用 VPN 连接就能访问所选的服务器和工作站。 
 
二、TS 网关的一些主要好处如下：    
1. 使远程用户能通过 Internet 安全连接到企业网络资源，而无需复杂的虚拟专用网络 (VPN) 连接。   
2. 利用 HTTPS 协议的安全性和可用性提供终端服务，无需进行客户端配置。   
3. 提供了一个全面的安全配置模型，管理员可使用此模型控制对网络中特定资源的访问。    
4. 使用户可穿过防火墙和网络地址转换器 (NAT) 远程连接到终端服务器和远程工作站。   
5. 提供了一个更加安全的模型，此模型允许用户仅访问所选的服务器和工作站，而不是通过 VPN 访问整个企业网络。    
 
终端服务网关为组织提供了一种安全、轻松的方法，使远程用户无需安装和配置 VPN 连接即可访问网络中的服务器和工作站。使用全面的安全功能，管理员还可以控制对特定资源的访问。

三、在外网与内网不通信的情况下，本文利用网关服务器来进行外网连接内网桌面。这类连接方式通常会信息安全、远程办公、网络隔离等场景使用。

本文以192.168.5.x为外网，192.168.4.x为内网。
云终端IP地址为外网/
  DoraCloud（192.168.4.167），虚拟桌面为内网/
   网关服务器为两个网卡，分别是192.168.5.17；192.168.5.17

组网图如下
 
![](./images/gateway_remote_service15.png.png) 


### 步骤一、安装 Windows Server 2012 R2 的步骤
　　1、使用 Windows Server 2012 R2 产品 CD 启动计算机。  
　　2、当提示输入计算机名称时，键入 2012RDG。  
　　3、按照屏幕上显示的其他说明完成安装。  
　
### 步骤二、设置静态IP
　　1、使用 2012RDG\Administrator 帐户登录到 2012RDG。两个网卡，同时插入内网和外网，内网（192.168.4.17）外网（192.168.5.17） 依次单击「开始」、"控制面板"、"网络和 Internet"、"网络和共享中心"、"更改适配器设置"，右键单击"本地连接"，然后单击"属性"。 如图下

 ![](./images/gateway_remote_service17.png.png) 
 ![](./images/gateway_remote_service18.png.png) 
 ![](./images/gateway_remote_service19.png.png) 

　　2、使用 2012RDG\Administrator 帐户登录到 2012RDG。 有域环境先加入域。
  ![](./images/gateway_remote_service16.png.png) 

3、检测IP静态IP是否修改正确。

### 步骤三、安装远程桌面网关服务
　　若要安装和配置远程桌面网关 服务器，必须添加远程桌面网关 角色服务。Windows Server 2012 R2 包含通过 服务器管理器 安装远程桌面网关 角色服务的选项。  

　1、以 2012RDG\Administrator 身份登录到 2012RDG。  
　2、点击左下角服务器图标，打开“服务器管理器”。  
  ![](./images/gateway_install1.png)

　　3、点击"添加角色"，如下图
  ![](./images/gateway_install2.png)

　　4、在"添加角色向导"中，如果显示"开始之前"，“安装类型”，“服务器选择”页，默认点击"下一步"。
  ![](./images/gateway_install3.png)

5、在"服务器角色"页上选择“选择桌面服务”，点击下一步。
![](./images/gateway_install4.png)

6、在“功能”，“远程桌面服务器”页面上默认点击“下一步”，进入到“角色服务”页面
![](./images/gateway_install5.png)
![](./images/gateway_install6.png)

7、在"角色服务"页上，选择“远程桌面网关”，选中时会弹窗框，点击“添加功能”即可。   
![](./images/gateway_install7.png)

全部安装完点击下一步,如下图：  
![](./images/gateway_install8.png)

8、后续操作一直点击“下一步”，一直到“确认”页面点击“安装”即可，等进度条完成后，重启服务器。  
![](./images/gateway_install9.png)
![](./images/gateway_install10.png)
　
### 步骤四、RD网关管理器配置

进去到“服务器管理器”,选中“所有服务器”->选中服务器列表中的服务器右键点击-》点击“RD网关管理器”，如下图
![](./images/gateway_remote_service1.png)  

**给服务器安装证书**  
 a) RD网关管理器->服务器->"查看或修改证书属性"。
  ![](./images/gateway_remote_service2.png)  
 b)创建并导入证书，完成后点击“应用”，注意，一定要是ip为证书名称，不然不会解析，例如外网ip192.168.5.17。在点击“确定”关闭弹窗，并备份证书用于windows远程连接。
 ![](./images/gateway_remote_service20.png.png)  
   ![](./images/gateway_remote_service4.png)  

 c)在服务器场选项，添加服务器场，输入外网IP地址，例如192.168.5.17.然后点“添加”按提示点“应用”等系统验证后，如图所示。
   ![](./images/gateway_remote_service21.png.png)  

**创建连接授权**  
a)点击“创建连接授权策略”，如下图
   ![](./images/gateway_remote_service6.png)  

b)设置策略名称。如下图  
 ![](./images/gateway_remote_service7.png)  

c)添加组成员或域用户组 注意！一定要添加登录用户组或添加域用户组。如下图：     
   ![](./images/gateway_remote_service22.png.png) 
注意：本文为了方便是直接添加管理员。项目可根据需要添加

d)可根据项目实际情况需要设置设备重定向跟超时。本文不设置。  
   ![](./images/gateway_remote_service24.png.png)

**创建“资源授权策略”**  

定义策略名称  
 ![](./images/gateway_remote_service10.png)  
 ![](./images/gateway_remote_service11.png)   
用户组添加用户组成员或域用户组，注意！一定要添加登录用户组或添加域用户组。如下图  
   ![](./images/gateway_remote_service23.png.png) 
 
指定网络资源，可根据项目实际设置。本文是设置"允许用户连接到任务网络资源（可根据项目具体设置）"  


### 步骤五、网关端口映射

a)这一步很重要，在网关服务器上做端口映射。将deskpool的443端口映射到网关的943端口。例如网关服务器为192.168.5.17，deskpool，服务器为
192.168.4.167，如下图：
 ![](./images/gateway_remote_service10.png.png) 
b)输入命令后检查一下端口映射，如下图
 ![](./images/gateway_remote_service11.png.png) 

### 步骤六、DoraCloud连接配置网关
1、登录Deskpool系统，进入“系统”->"网关"填写网关，完成后点击设置
![](./images/gateway_remote_service43.png.png)

2、本文环境是需要经过网关。如果内网不需要经过网关，可以设置过滤网段，如下过滤192.168.4.*的IP不需经过网关。  
![](./images/gateway_remote_service44.png.png)


### 步骤七、通过网关连接内网桌面

#### 方法1：云终端通过网关连接内网
云终端连接是不需要安装证书，所以直接进行连接

1、点击连接deskpool，输入服务器与账户密码。如下图
 ![](./images/gateway_remote_service12.png.png)
 ![](./images/gateway_remote_service13.png.png)

2、连接进入桌面
 ![](./images/gateway_remote_service14.png.png)   

#### 方法2：DoraCloud客户端通过网关登录


1、首先将网关服务器创建的证书拷贝到windows内安装。证书的路径一般会创建在/我的电脑、文档的目录下。如图
   ![](./images/gateway_remote_service25.png.png)
2、将证书安装在windows上，如图下步骤
 ![](./images/gateway_remote_service26.png.png)
 ![](./images/gateway_remote_service27.png.png)
 ![](./images/gateway_remote_service28.png.png)
3、打开DoraCloud客户端，输入服务器地址和用户名及密码。如图下步骤
![](./images/gateway_remote_service31.png.png)
![](./images/gateway_remote_service30.png.png)
4、会自动弹出RDP连接，点击是，进入桌面。如图下
![](./images/gateway_remote_service32.png.png)
![](./images/gateway_remote_service33.png.png)
![](./images/gateway_remote_service34.png.png)

#### 方法3：浏览器通过网关连接内网
1、首先也是先将证书安装完成
2、安装完成后在浏览器输入DoraCloud服务器。如https://192.168.5.17:943,并点击转入此网页。如图下
![](./images/gateway_remote_service35.png.png)
3、输入用户及密码，会自动下载RDP连接包。步骤如图所示
![](./images/gateway_remote_service36.png.png)
![](./images/gateway_remote_service37.png.png)
![](./images/gateway_remote_service38.png.png)
![](./images/gateway_remote_service39.png.png)
4、再次弹出输入密码，如图下操作进入桌面
![](./images/gateway_remote_service40.png.png)
![](./images/gateway_remote_service41.png.png)
![](./images/gateway_remote_service42.png.png)