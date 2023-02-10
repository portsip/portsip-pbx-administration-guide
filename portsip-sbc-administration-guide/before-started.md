# 开始前
>PortSIP SBC不能与PortSIP PBX v12.x安装在同一台服务器上。它可以和PortSIP PBX v16.x安装在同一台服务器上。
  
## Linux的前提知识
在Linux环境下部署PortSIP SBC需要计划和了解会话发起协议（SIP）、音频和视频呼叫、存在和即时通讯（IM）管理。  
  
你还应该具备以下Linux基础设施的知识，一个流行的Linux发行版：   
+ CentOS 7.9 (64位)
+ Debian 10.x, 11.x (64位)
+ Ubuntu 18.04, 20.04, 或 22.04 (64位)
+ Docker 20.10或更高版本。
+ IPv4/IPv6
+ Systemd
+ IP tables
+ 防火墙
+ HTTP  
  
本文假设Linux操作系统已经部署完毕，并且管理员已经获得了Linux的root权限。  
如果你的 Linux 服务器已经安装了 **PortSIP PBX v12.x**，请在安装 PortSIP SBC 之前遵循以下步骤。这些步骤将删除你所有的 PBX 数据。你可以先备份 `/var/lib/portsip` 目录。
```
sudo docker stop portsip-pbx
sudo docker rm portsip-pbxcd 
cd /var/lib/ && sudo rm -rf portsip
```  
  
上面的命令删除了PBX的`docker instance`。现在让我们列出PortSIP PBX v12.x的`docker image`。  
```
// This command will list all docker images
sudo docker image list

// You can use the docker image rm command to delete the PortSIP PBX image, the "1a0c"
// is the first 4 characters of the image tag id.
sudo docker image rm 1a0c
```  
  
## Windows的先决条件知识
在 Windows 环境中部署 PortSIP SBC 需要规划和了解会话启动协议（SIP）、音频和视频呼叫、存在和即时通讯（IM）管理。您还应该具备以下Windows基础设施的知识。  
  
Windows桌面或Windows服务器操作系统：   
+ Windows 10/11 (64位)
+ Windows服务器2016/2019/2020/2022
+ IPv4/IPv6
+ Windows 防火墙  

本文假设Windows操作系统已经部署完毕，并且管理员已经获得了Windows的管理员权限。  
  
如果您的Windows服务器已经安装了**PortSIP PBX v12.x**，请在安装PortSIP SBC之前按照以下步骤进行操作。这些步骤将删除你所有的PBX数据。你可以先备份数据目录：`C:\ProgramData\PortSIP` 。   
1. 卸载 PortSIP PBX v12.x
2. 删除 `C:\ProgramData\PortSIP` 目录。
3. 删除 `C:\Program Files\PortSIP` 目录。  

## 支持的云和虚拟化环境
为了建立一个高可用性的通信解决方案，帮助客户降低成本，提高通信性能，PortSIP SBC承诺支持云服务，并确认与以下云和虚拟化环境兼容：   
+ VMware ESX 5.X及以上版本。
+ Linux HyperV
+ Microsoft HyperV 2016 R2及以上版本
+ Microsoft AZURE
+ Amazon AWS
+ Google Could
+ 数字海洋
+ 阿里云  

## 其它需求   
+ 最新的Firefox, Google Chrome, Edge浏览器
+ Microsoft.NET Framework 4.5或更高版本
+ Linux和Linux Internet管理知识
+ 对Windows和Windows互联网管理有一定了解
+ 一个持续的互联网连接到stud4.l.google.com，端口为19302。
+ 确保服务器日期时间同步正确。
## 支持FQDN
尽管PortSIP PBX的设计是能够在没有指定FQDN的服务器上运行，但我们建议指定FQGN，有以下好处：   
+ 更容易访问 PortSIP PBX 的Web Portal
+ 支持Microsoft Teams的直接路由
+ 在访问Web Portal时，可以方便地访问HTTPS
+ 避免在访问WebRTC客户端时出现浏览器警告  
  
你所使用的FQDN必须能够被正确地解析到安装有PortSIP PBX的局域网服务器上。如果PortSIP PBX安装在公共网络上，FQDN必须能够正确解析到安装有PBX的服务器的公共网络地址。
## 准备TLS证书
当需要对SIP通信进行额外的安全保护时，传输层安全（TLS）被用来保护设备的SIP信令连接。在TLS协议中，数据被加密和保护。TLS通信需要证书来验证安全数据的接收者。  
  
PortSIP PBX和SBC使用证书来提供安全的SIP流量和服务，并支持WebRTC和Microsoft Teams Direct Routing。此外，证书被用来提供对设备的安全访问，以及网络（HTTPS）会话。  
  
请阅读[本主题](https://support.portsip.com/tutorials/preparing-tls-certificates-for-tls-https-webrtc)以准备一个可信的SSL证书。
## 获得帮助和支持资源
[PortSIP官方支持社区](https://forum.portsip.com/)  
[PortSIP 支持中心](https://support.portsip.com/)  
[提交请求](https://portsip.zendesk.com/hc/en-us/requests/new)  
[邮件支持](mailto:support@portsip.com)
