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
  
请仔细阅读[支持的 Linux 操作系统](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#supported-linux-os)、[为安装准备 Linux 主机](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#preparing-the-linux-host-machine-for-installation)、[为 TLS/HTTPS/WebRTC 准备 TLS 证书](https://support.portsip.com/tutorials/preparing-tls-certificates-for-tls-https-webrtc)。
  
## 9.2.1 安装PortSIP PBX和SBC
假设PBX已经按照[1安装了PortSIP PBX](https://support.portsip.com/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx)。  
要安装SBC，请按照以下步骤进行。
  
**安装PortSIP SBC for Linux**  
  
+ 执行下面的命令，下载SBC的安装脚本。  
```
mkdir -p /opt/portsip
cd /opt/portsip && rm -f *.sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/install_sbc_docker.sh     -o  install_sbc_docker.sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/sbc_ctl.sh        -o  sbc_ctl.sh
```
执行下面的命令来安装Docker-Compose环境。 如果得到像*** cloud.cfg (Y/I/N/O/D/Z) [default=N] 这样的提示，输入Y，然后按回车键。
```
cd /opt/portsip
/bin/sh install_sbc_docker.sh
```
+ 下面的命令用于在服务器上创建和运行SBC。
```
cd /opt/portsip
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```
**安装PortSIP SBC for Windows**
  
你可以在PortSIP网站上下载PortSIP SBC的安装程序，只需双击安装程序并按照说明进行安装。
  
## 9.2.2 配置PortSIP SBC   
1. 按照[准备TLS/HTTPS/WebRTC的SSL证书指南](https://support.portsip.com/tutorials/preparing-tls-certificates-for-tls-https-webrtc)准备SSL证书。
2. 在浏览器中进入https://uc.portsip.cc:8883 ，并使用凭证admin/admin登录。如果浏览器显示SSL证书警告，请忽略该警告并继续处理。
3. 从菜单中选择 "设置 > TLS证书"，点击添加按钮，在 "描述 "栏中输入 "SBC主机名称 "为例；在 "TLS域 "中输入uc.portsip.cc。在Windows记事本中打开 "portsip.pem "文件，将其内容复制到 "Certificate Context "字段。复制并粘贴 "portsip.key" 文件的内容到 "Private Key Context" 字段。点击 "确定 "按钮，保存证书。
4. 从菜单中选择 "设置 > 网络"，然后在 "网络域 "字段中填写uc.portsip.cc，"私有IPv4 "填写192.168.1.72，公共IPv4填写66.175.221.120。默认情况下，"自动创建默认传输 "选项是打开的，SBC 将在成功设置 SBC IP 地址后创建默认传输。
5. 默认的传输方式。
 + 5069端口的TCP：用于与PBX通信
 + 5067端口的TLS：用于与Microsoft Teams通信
 + 5065端口的WSS：用于提供WebRTC服务。
 + 5066端口的UDP：用于提供正常的SIP服务。
你可以关闭这个选项，防止SBC自动创建默认的传输，但不建议这样做。
6. 当你点击确定按钮时，SBC将自动重新启动，并立即签出你的名字。
7. 在服务器上执行下面的命令。如果服务器是Windows，直接重启服务器即可。
```
cd /opt/portsip
/bin/sh sbc_ctl.sh restart
```
8. 登录PBX网络门户https://uc.portsip.cc:8887 ，点击菜单 "高级>SBC"。点击 "生成 "按钮，生成SBC的访问令牌。点击 "复制 "按钮，复制该令牌。
9. 登录PortSIP SBC网络门户https://uc.portsip.cc:8883 。从菜单中选择设置 > PBX。你必须在这里设置PBX的信息，然后SBC才能和PBX进行通信。将复制的令牌粘贴到 "PBX Access Token "区域，并在 "PBX IPv4 Address "区域输入PBX的私有IP 192.168.1.72。由于PBX的TCP传输是在5063端口创建的，所以请在 "与PBX通信的首选传输方式 "中选择TCP，并在 "PBX端口 "中输入 "5063"。
10. 在菜单中选择 "设置 > 传输"。SBC要求在这里添加三种类型的传输。一个是WebRTC客户端，一个是Microsoft Teams，还有一个是SBC与PBX的通信。在完成上述第4步，打开 "自动创建默认传输 "选项后，SBC将自动创建默认传输。这些默认的传输方式不建议被改变。
11. 如果你一直使用默认传输，这一步就没有必要。如果你想创建你自己的传输，你可以删除现有的传输。
+ 添加 "TCP "传输，用于SBC与PBX的通信，请参考下面的截图，并在 "网络接口 "中选择 "SBC专用IP"。
+ 为WebRTC客户端添加 "WSS "传输，如下图所示，"网络接口 "请选择 "SBC公共IP"。
+ 为SBC添加 "TLS "传输，以便与Microsoft Teams通信，如下图所示，请在 "网络接口 "中选择 "SBC公共IP"。
+ 为SBC添加一个 "UDP "传输，以提供正常的SIP服务，如下图所示，请选择 "SBC公共IP "作为 "网络接口"。
![image](https://user-images.githubusercontent.com/112454775/217769107-d74663f9-8619-4f30-a46a-d3a464ade6b4.png)
![image](https://user-images.githubusercontent.com/112454775/217769122-27086ce4-8716-4dc4-8dd2-ec0bb95812ee.png)
12. 如果你在步骤11中创建了自己的传输，而不是使用默认的传输，请用下面的命令在防火墙上打开你的传输端口。例如，如果你在5066端口创建了UDP传输，5069端口创建了TCP传输，5067端口创建了TLS传输，5065端口创建了WSS传输，请执行以下命令。
```
firewall-cmd --permanent --service=portsip-sbc --add-port=5066/udp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5065/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5067/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5069/tcp --set-description="PortSIP SBC"
firewall-cmd --reload
```
13. 在浏览器中打开网址https://uc.portsip.cc:8883/webrtc ，WebRTC客户端将被启动。只需输入分机号码、密码和租户的SIP域名，就可以在PBX上注册，拨打和接收电话。
> Once added a new transport, you have to change the firewall rule to allow that transport port. The client app, IP Phone will reach PBX by transport and port.
  
# 9.3 在不同的服务器上安装PortSIP SBC和PBX
通常情况下，PortSIP SBC与PortSIP PBX分别部署在不同的服务器上，SBC位于前面，PBX对终端用户是透明的。
  
假设我们要安装 PortSIP PBX 和 SBC，配置如下。
  
+ PBX服务器的私有IP是192.168.1.72。
+ SBC服务器的私有IP是192.168.1.73，公共IP是66.175.221.120
+ 域名 sbc.portsip.cc 已被解析到 SBC 服务器的公共 IP 66.175.221.120。
+ 域名uc.portsip.cc已经被解析到PBX服务器192.168.1.72，这一步没有必要。
+ 为域名 portsip.cc 提供一个可信的通配符 SSL 证书
  
请仔细阅读[支持的 Linux 操作系统](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#supported-linux-os)、[为安装准备 Linux 主机](https://support.portsip.com/portsip-pbx-user-guide/portsip-pbx-administration-guide-v16.x/1-installation-of-the-portsip-pbx#preparing-the-linux-host-machine-for-installation)、[为 TLS/HTTPS/WebRTC 准备 TLS 证书](https://support.portsip.com/tutorials/preparing-tls-certificates-for-tls-https-webrtc)。


