# 6 传输协议管理

完成 "设置向导 "后，您可以在门户网站中管理您的PortSIP PBX。

PortSIP PBX 支持多种传输方式，包括 UDP，TCP 和 TLS，用于 SIP 信令。您需要配置传输方式，并设置监听 SIP 信令时使用的端口。

```
只有系统管理员被允许创建或删除SIP传输。当删除时，至少要留下一个传输。
```

默认的传输已经通过 "设置向导 "进行了配置。要进行更改，您需要以 "系统管理员 "的身份登录 PBX Web 门户，选择 "呼叫管理器 > 传输 "菜单。点击 "添加 "按钮，添加一个新的传输。

```
一旦你添加了一个新的传输，你必须改变防火墙规则以允许该传输端口。IP电话客户端应用程序将通过传输和端口连接到PBX。

如果PBX运行在云平台上，比如AWS，并且云平台有自己的防火墙，你也必须在云平台的防火墙上打开端口。
```

## 6.1 添加 UDP/TCP 传输协议

要添加UDP/TLS/TCP传输：

+ 点击 "添加 "按钮，并在 "传输协议 "框中选择UDP/TLS/TCP。UDP/TLS/TCP的默认传输端口是5060/5061/5063。你可以随心所欲地指定另一个端口，但该端口必须不被其他应用程序所使用

+ 点击 "确定 "按钮来添加传输。

## 6.2 添加 TLS 传输协议

首先，准备好证书文件。请参考 "Prepare TLS Certificates for TLS/HTTPS/WebRTC"，为你的PBX "Web Domain "从可信的第三方供应商处购买证书文件（假设你购买的是uc.portsip.cc这个域名的证书）。

点击 "添加 "按钮，在 "传输协议 "框中选择 "TLS"。TLS 的默认传输端口是 5061。你可以根据自己的需要指定另一个端口，但该端口不能被其他应用程序使用。

点击 "确定 "按钮，添加传输。

## 6.3 配置防火墙规则

如果你创建了自己的传输，而不是使用默认的传输，你必须用下面列出的命令打开防火墙上的传输端口。

例如，如果你在5066端口创建了UDP传输，在5071端口创建了TCP传输，在5072端口创建了TLS传输，在5075端口创建了WSS传输，请执行以下命令。
```
firewall-cmd --permanent --service=portsip-pbx --add-port=5066/udp--set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5071/tcp--set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5072/tcp--set-description="PortSIP PBX"
firewall-cmd --permanent --service=portsip-pbx --add-port=5075/tcp--set-description="PortSIP PBX"
firewall-cmd --reload
```
```
如果PBX运行在云平台上，如AWS，并且云平台有自己的防火墙，你也必须打开云平台防火墙上的端口。
```





























































































