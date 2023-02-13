# 1 安装PortSIP SBC
## 1.1 下载 PortSIP SBC
[PortSIP SBC](https://www.portsip.com/portsip-sbc/)最新的免费版本总是可以在[PortSIP Website](https://www.portsip.com/download-portsip-sbc/)上找到并下载。它适用于64位Windows和Linux，但不适用于32位版本。  
免费版的[PortSIP SBC](https://www.portsip.com/portsip-sbc/)最多可提供3个同时通话。如果你需要更多的同时通话，请参考[PortSIP SBC Pricing Page](https://www.portsip.com/portsip-sbc-pricing/)以获得更多细节。  
  
下载完成后，你会得到安装程序。
## 1.2 在Linux上安装PortSIP SBC
### 支持的Linux操作系统   
+ CentOS 7.9
+ Ubuntu 18.04, 20.04, 22.04
+ Debian 10.x, 11.x  

它只支持64位操作系统。  
  
### 准备安装的Linux主机
在安装PortSIP SBC之前，必须完成的任务。   
+ 如果将安装SBC的Linux位于一个局域网内，为SBC服务器分配一个`静态的局域网IP地址`；如果它在一个公网上，为SBC服务器分配一个`静态的内网IP地址`和一个`静态的公网IP地址`。
+ 在安装PortSIP SBC之前，请安装所有可用的更新和服务包。
+ 确保你的系统和网络适配器的所有节能选项被禁用（通过将系统设置为高性能）。
+ 不要在主机上安装TeamViewer、VPN和其他类似软件。
+ PortSIP SBC不能安装在作为DNS或DHCP服务器的主机上。
+ 您的防火墙必须允许以下端口。   
   - UDP：5066，25000-35000
   - TCP: 5065, 5067, 8882, 8883.请同时确保上述端口没有被其他应用程序使用。
+ 确保服务器的日期时间是正确同步的。
必须以root用户的身份执行所有的Linux命令，请先`su root`。  
>如果SBC运行在AWS等云平台上，并且云平台有自己的防火墙，你也必
### 步骤1 下载安装脚本
执行下面的命令，下载安装脚本。  
```
mkdir -p /opt/portsip
cd /opt/portsip
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/install_sbc_docker.sh     -o  install_sbc_docker.sh
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v16.x/sbc_ctl.sh        -o  sbc_ctl.sh
```
### 步骤2 设置Docker环境
执行下面的命令来安装`Docker-Compose`环境。如果你得到像`*** cloud.cfg (Y/I/N/O/D/Z) [default=N]` 这样的提示，输入**Y**，然后按**回车键**。
```
cd /opt/portsip
/bin/sh install_sbc_docker.sh
```
### 步骤3 创建并运行PortSIP SBC Docker容器实例
下面的命令用于在服务器上创建和运行SBC。  

```
cd /opt/portsip
/bin/sh sbc_ctl.sh run -p /var/lib/portsip -i portsip/sbc:10
```
现在你可以通过[ https://server-ip-or-domain:8883](https://server-ip-or-domain:8883) 访问 SBC Web Portal，默认的系统管理员名称和密码是`admin`。  
  
成功创建SBC docker实例后，你可以使用下面的命令来管理它。
#### 显示SBC Docker实例的状态。
```
cd /opt/portsip && /bin/sh sbc_ctl.sh status
```
#### 启动 SBC Docker 实例。
```
cd /opt/portsip && /bin/sh sbc_ctl.sh start
```
#### 停止 SBC Docker 实例。
```
cd /opt/portsip && /bin/sh sbc_ctl.sh stop
```
#### 重新启动 SBC Docker 实例。
```
cd /opt/portsip && /bin/sh sbc_ctl.sh restart
```
#### 删除 SBC Docker 实例。
```
cd /opt/portsip && /bin/sh sbc_ctl.sh rm
```
## 1.3 在Windows上安装PortSIP SBC
### 支持的Windows操作系统
+ Windows 10, 11
+ Windows Server 2016, 2019, 2022  
它仅支持64位操作系统。  

### 准备安装的Windows主机
安装PortSIP SBC之前必须完成的任务。   
+ 如果将安装SBC的Linux位于一个局域网内，为SBC服务器分配一个`静态的局域网IP地址`；如果它在一个公网上，为SBC服务器分配一个`静态的内网IP地址`和一个`静态的公网IP地址`。
+ 在安装PortSIP SBC之前，请安装所有可用的Windows更新和服务包。安装Windows更新后的重新启动可能会发现额外的更新。在运行PortSIP PBX安装之前，要特别注意安装所有Microsoft .Net的更新。
+ 防病毒软件不应该扫描以下目录，以避免复杂化和写访问延迟。`C:\Program Files\PortSIP`; `C:\Programdata\PortSIP`
+ 不要在您的PortSIP SBC服务器上安装VPN、TeamViewer软件。
+ 确保`Windows Firewall`服务已经启动。
+ 确保你的系统和网络适配器的所有节能选项被禁用（通过将系统设置为高性能）。
+ 如果是Windows客户端PC，请禁用蓝牙适配器。
+ PortSIP SBC不能安装在作为DNS或DHCP服务器的主机上，也不能安装MS SharePoint或Exchange服务。
+ 您的防火墙必须允许以下端口。
   + UDP：5066，25000-35000
   + TCP: 5065, 5067, 8882, 8883. 也请确保上述端口没有被其他应用程序使用。
+ 确保服务器日期时间同步正确。
+ 确保你的Windows防火墙被启用。  
>如果SBC运行在云平台上，如AWS，并且云平台有自己的防火墙，你也必须在云平台的防火墙上打开端口。
### 第1步 安装新的PortSIP SBC for Windows
SBC安装程序可以在[PortSIP Website](https://www.portsip.com/download-portsip-sbc/)上下载。  
要安装PortSIP SBC，您只需要双击安装程序，它将指导您完成安装过程。  
安装成功后，PortSIP SBC服务将自动启动（此后每次计算机启动时也是如此）。  
### 第2步：配置Windows防火墙规则
在成功安装了PortSIP SBC之后，PortSIP SBC的端口已使用Windows防火墙打开。  

如果您的服务器有一个防火墙，阻挡了端口，您必须打开以下端口，以便使PortSIP SBC正常工作。  
+ 以下端口必须是您的防火墙所允许的。
   + UDP: 5066, 25000-35000
   + TCP: 5065, 5067, 8882, 8883. 也请确保上述端口没有被其他应用程序使用。
>如果SBC运行在云平台上，如AWS，并且云平台有自己的防火墙，你也必须在云平台的防火墙上打开端口。  
现在你可以通过[https://server-ip-or-domain:8883](https://server-ip-or-domain:8883)，访问SBC Web portal，默认的系统管理员名称和密码是`admin`。
## 1.4 卸载 PortSIP SBC
请使用下面的步骤来卸载用于Linux的PortSIP SBC。  
### 卸载SBC
```
cd /opt/porsip
/bin/sh sbc_ctl.sh stop
/bin/sh sbc_ctl.sh rm
cd /var/lib/portsip/
rm -rf sbc
```
对于 Windows 的安装，只需从 Windows 控制面板上卸载即可。
