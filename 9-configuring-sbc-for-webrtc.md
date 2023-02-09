PortSIP PBX推出了PortSIP SBC，它作为一个组件与提供WebRTC服务的PortSIP PBX一起工作；支持Microsoft Teams Direct Routing。  
PortSIP SBC为PortSIP PBX提供灵活性和可扩展性，使我们能够支持大量的WebRTC客户。  
在我们开始之前，请阅读"[什么是SBC?](https://support.portsip.com/tutorials/what-is-an-sbc)"。通常情况下，SBC的概念如下所示。  
![image](https://user-images.githubusercontent.com/112454775/217741986-8f856139-71fa-4ad8-ad4d-437cdd3daf31.png)
# 9.1 部署方案  
**带有SIP中继线和互联网用户的企业IP PBX**  
本例介绍了在IP PBX、SIP中继和游牧互联网用户（应用程序、WebRTC、IP电话）之间互通时，如何配置SBC。该示例场景包括以下拓扑结构。  
+ 企业局域网IP PBX的IP地址，192.168.1.72  
+ 一个互联网SIP中继线  
+ 游牧的互联网用户  
+ 微软团队  
![image](https://user-images.githubusercontent.com/112454775/217743875-fe77e3d1-e95b-4bac-b65d-b4b59f5a1394.png)
SBC逻辑网络接口连接
+ 一个逻辑网络与LAN对接，使用IP地址192.168.1.73。该接口也用于管理。
+ 一个逻辑网络与DMZ/WAN对接，使用IP地址66.175.221.120。
SBC物理局域网端口的连接
+ 一个以太网端口连接到LAN。
在上述拓扑结构中，SBC只有一个物理网络接口，提供两个逻辑网络接口；对于DMZ/WAN接口，是IP 66.175.221.120的虚拟映射。
PortSIP SBC也可以用两个物理网络接口进行部署。  
![image](https://user-images.githubusercontent.com/112454775/217744450-8666c4a1-6b3e-473f-a17f-8f888db98831.png)
SBC逻辑网络接口连接
+ 一个逻辑网络与LAN对接，使用IP地址192.168.1.73。该接口也用于管理。
+ 一个逻辑网络与DMZ/WAN对接，使用IP地址66.175.221.120。
SBC物理局域网端口的连接
+ 一个以太网端口连接到局域网。
+ 一个以太网端口连接到DMZ / WAN。  
  
**托管的云计算PBX**  
本例介绍了在局域网IP电话/应用/WebRTC客户端和托管云IP PBX之间互通时，如何配置PortSIP SBC。这个例子的场景包括以下拓扑结构。   
![image](https://user-images.githubusercontent.com/112454775/217745799-7e804c3d-9312-42d9-94da-cec1504a90a2.png)  
SBC的逻辑网络接口连接  
这个例子采用了两个逻辑网络接口，一个使用IP地址172.31.3.190。该接口用于与位于同一局域网的PBX（172.31.3.192）进行通信。一个是使用IP地址185.53.179.172，提供SIP服务、WebRTC服务和Microsoft Teams Direct Routing。在局域网中，IP电话/应用程序/WebRTC客户端使用SIP和RTP/SRTP与SBC通信。
# 9.2 在同一台服务器上安装PortSIP SBC和PBX
PortSIP SBC可以和PortSIP PBX一起部署在同一台服务器上；在这种情况下，PBX直接提供SIP呼叫，SBC提供WebRTC服务，也为Microsoft Teams直接路由提供互通。  
  
假设我们想在以下服务器配置上部署PortSIP PBX和SBC。  
  
+ 服务器的私有IP是192.168.1.72，公共IP是66.175.221.120。
+ 域名uc.portsip.cch已经被解析到公共IP的66.175.221.120。
+ 域名uc.portsip.cc的可信SSL证书
  
请仔细阅读[《支持的 Linux 操作系统》](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#supported-linux-os)、[《为安装准备 Linux 主机》](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#preparing-the-linux-host-machine-for-installation)、[《为 TLS/HTTPS/WebRTC 准备 TLS 证书》](https://support.portsip.com/tutorials/preparing-tls-certificates-for-tls-https-webrtc)。
  
## 9.2.1 安装PortSIP PBX和SBC
假设PBX已经按照[1安装了PortSIP PBX](https://support.portsip.com/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx)。  
要安装SBC，请按照以下步骤进行。
  
**安装PortSIP SBC for Linux**  
  
执行下面的命令，下载SBC的安装脚本。  
```
mkdir -p /opt/portsip
cd /opt/portsip && rm -f *.sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/install_sbc_docker.sh     -o  install_sbc_docker.sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/sbc_ctl.sh        -o  sbc_ctl.sh
```


