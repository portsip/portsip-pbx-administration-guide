# 开始前
```PortSIP PBX v16.0 是一个全新的主要版本，这意味着它不能从 PortSIP PBX v12.x 升级。16.0 版本必须安装在一个没有安装 v12.x 的新服务器上。```  
## Linux的前提知识
在Linux环境下部署PortSIP PBX需要计划和了解会话发起协议（SIP）,音频和视频呼叫，存在和即时通讯（IM）管理。  
  
你还应该具备以下Linux基础架构和流行的Linux发行版的知识：   
+ CentOS 7.9 (64位)
+ Debian 10.x, 11.x (64位)
+ Ubuntu 18.04, 20.04, 或 22.04 (64位)
+ Docker 20.10或更高版本。
+ IPv4/IPv6
+ Systemd
+ IP表
+ Firewalld
+ HTTP  
  
本文假设已经部署了Linux操作系统，并且PortSIP PBX的管理员已经获得了Linux的**root权限**。  
  
如果您的 Linux 服务器已经安装了 **PortSIP PBX v12.x**，请在安装 v16.0 之前按照以下步骤进行操作。这些步骤将删除你所有的 PBX 数据。你可以先备份` /var/lib/portsip `目录。  
  
```
sudo docker stop portsip-pbx
sudo docker rm portsip-pbx
cd /var/lib/ && sudo rm -rf portsip
```

上面的命令删除了PBX v12.x的`docker instance`。现在让我们列出PortSIP PBX v12.x的`docker image`。  
  
```
// This command will list all docker images
sudo docker image list

// You can use the docker image rm command to delete the PortSIP PBX image, the "1a0c"
// is the first 4 characters of the image tag id.
sudo docker image rm 1a0c
```
  
## 具备Windows的先决条件
在Windows环境下部署PortSIP PBX需要计划和了解会话发起协议（SIP）、音频、视频通话、存在和即时通讯（IM）管理。您还应该了解以下Windows基础结构。  
  
Windows桌面或Windows服务器操作系统：   
+ Windows 10/11 (64位)
+ Windows服务器2016/2019/2020/2022
+ IPv4/IPv6
+ Windows 防火墙  
  
本文假设Windows操作系统已经部署完毕，并且PortSIP PBX的管理员已经获得了Windows的管理员权限。  
  
如果您的 Windows 服务器已经安装了 **PortSIP PBX v12.x**，请在安装 v16.0 之前按照以下步骤进行操作。这些步骤将删除你所有的 PBX 数据。你可以先备份`C:\ProgramData\PortSIP `目录。   
1. 卸载 PortSIP PBX v12.x
2. 删除 `C:\ProgramData\PortSIP` 目录
3. 删除 `C:\Program Files\PortSIP` 目录  
## 支持云和虚拟化环境
为了建立一个高可用性的通信解决方案，帮助客户降低成本，提高通信性能，PortSIP PBX承诺支持云服务，并确认与以下云和虚拟化环境兼容：   
+ VMware ESX 5.X及以上版本。
+ Linux HyperV
+ 微软HyperV 2016 R2及以上版本
+ 微软AZURE
+ 亚马逊AWS
+ Google Could
+ 数字海洋
+ 阿里云
## 系统性能取决于以下关键因素   
+ PBX所需的最大同时通话量
+ PBX所需的最大在线用户数
+ 通话录音
+ 仅录制音频或同时录制音频和视频
+ PBX上用于音频/视频会议的最大在线用户数
+ PBX上最大的IVR（虚拟接待员）数。
+ PBX上最大的呼叫队列数
+ PBX上最大的铃声组数  
  
根据以上列出的主要功能，PortSIP PBX能够在PC和服务器上运行，其CPU从Intel i3 CPU到Xeon不等。
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
+ 在 PBX 的 IP 地址改变后，更容易管理 IP 电话和客户端
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
