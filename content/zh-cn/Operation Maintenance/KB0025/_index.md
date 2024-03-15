---
title: "KB0025.DoraCloud服务器配置SSL证书"
linkTitle: "KB0025.DoraCloud服务器配置SSL证书"
date: 2024-1-6
weight: 25
description: >
  DoraCloud管理系统增加SSL证书
---

通过浏览器访问DoraCloud云桌面系统时，浏览器会给出安全提示，
DoraCloud管理系统采用Java开发，运行在Tomcat容器中。整个管理打包在一个Linux虚拟机中。

给DoraCloud管理系统添加证书包括以下几个步骤：

 **1. 获取证书文件和对应的口令。 可以从相应的证书签发机构获得证书。** 

比如从腾讯云获得的免费SSL证书，选择适用于 tomcat 的pfx格式的证书，包括如下两个文件：
```
keystorePass.txt
vdi.doracloud.cn.pfx
```
 **2. 登陆DoraCloud Linux VM，默认的账号是 root  dora@cloud**  


 **3. 拷贝证书文件到   /home/jieyun/deskpool/tomcat/cert 目录** 
```
mkdir /home/jieyun/deskpool/tomcat/cert
cp vdi.doracloud.cn.pfx  /home/jieyun/deskpool/tomcat/cert
```

 **4. 修改配置文件** 

进入 /home/jieyun/deskpool/tomcat/conf/ ，编辑  server.xml ，修改Connector的定义为如下内容

```
        <Connector SSLEnabled="true" acceptCount="100" clientAuth="false"
            disableUploadTimeout="true" enableLookups="false" maxThreads="25"
            port="443" keystoreFile="cert/vdi.doracloud.cn.pfx" keystorePass="73xfg573dnhn1"
            protocol="org.apache.coyote.http11.Http11NioProtocol" scheme="https"
            secure="true" sslProtocol="TLS" />
```

注意，主要是修改  keystoreFile 和  keystorePass 两个属性的内容。   keystorePass的密码请从keystorePass.txt文件中复制过来。

`keystoreFile="cert/vdi.doracloud.cn.pfx" keystorePass="73xfg573dnhn1"` 


然后重启 DoraCloud Linux VM。

 **5. 重启完毕后，通过浏览器访问 DoraCloud管理系统，验证SSL前面已经启用。** 


