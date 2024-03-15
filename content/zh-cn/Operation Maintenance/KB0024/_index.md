---
title: "KB0024.rdp远程连不上排查步骤"
linkTitle: "KB0024.rdp远程连不上排查步骤"
date: 2024-1-6
weight: 24
description: >
  rdp远程连不上排查步骤
---




1. 确认目标计算机的状态
```
#在虚拟化平台pve或winserver查看虚拟机是否开机，正常运行

#虚拟化平台能否获取到虚拟机ip
```

2. 检查网络连接
```
#windows虚拟机开启ping功能
netsh firewall set icmpsetting 8    #此命令已经被官网弃用，但是依然有效

#windows虚拟机开启ping功能
netsh advfirewall firewall add rule name="ICMP Allow Incoming V4 Echo Request" protocol=icmpv4:8,any dir=in action=allow       #推荐使用

#验证是否能平通虚拟机
ping + 虚拟机ip

#能ping通的状态
C:\Users\kk>ping 192.168.1.173

Pinging 192.168.1.173 with 32 bytes of data:
Reply from 192.168.1.173: bytes=32 time<1ms TTL=128
Reply from 192.168.1.173: bytes=32 time<1ms TTL=128
Reply from 192.168.1.173: bytes=32 time<1ms TTL=128
Reply from 192.168.1.173: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.173:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

3. 检查rdp的3389端口是否被放行
```
#管理员权限打开cmd
netstat -an | find "3389"

#如果3389端口正在监听（LISTENING），那么它是开启的
C:\Users\Administrator>netstat -an | find "3389"
  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING
  TCP    [::]:3389              [::]:0                 LISTENING
  UDP    0.0.0.0:3389           *:*
  UDP    [::]:3389              *:*
```

4. 确认远程桌面服务状态
```
#Windows + R 组合键来打开运行对话框。

#输入 services.msc 并按下回车键

#检查服务应用程序，确保这两个服务正在运行：
远程桌面服务：Remote Desktop Services
远程桌面服务端口重定向程序：Remote Desktop Services UserMode Port Redirector

#如果一个或两个服务未运行，请启动它们
```

5. 允许远程连接到此计算机
```
#在虚拟机上，右键点击 "此电脑" 或 "计算机"，选择 "属性"，然后点击 "远程设置"。

#确保 "允许远程连接到此计算机" 选项已经选中。

#如果使用的是特定的用户或组进行 RDP 连接，请确保他们已经被授权远程访问
```

6. 防火墙和安全软件
```
#如果目标计算机有第三方防火墙或安全软件，确保它们没有阻止 RDP 连接。

#暂时禁用防火墙或安全软件进行测试，但请注意安全风险，并在测试完成后立即重新启用它们
```

7. RDP 客户端
```
尝试从另一台计算机或设备使用 RDP 连接到目标计算机，以排除客户端特定的问题
```